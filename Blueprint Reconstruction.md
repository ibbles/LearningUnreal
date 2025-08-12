Blueprint Reconstruction is the process of serializing all Components in a [[Blueprint Actor]] instance, deleting them, and then recreating them from the [[Blueprint Class]] definitions and the serialized data.
This happens whenever we change a [[Blueprint Actor]] instance from the [[Unreal Editor]] UI, including during [[Play In Editor]] sessions.
Any Component added with an Add TYPE Component [[Blueprint Node]] will be deleted and not recreated.

# `PostEditChange(Chain)?Property`

See also [[Property Changed Events]].

An effect of Blueprint Reconstruction is that `PostEditChange(Chain)?Property` becomes non-trivial to use.
For example, the following will not work as expected:
```cpp
void UMyComponent::PostEditChangeProperty(FPropertyChangedEven& Event)
{
	// This may recreate the component, making 'this' garbage.
	Super::PostEditChangeProperty(Event);

	// No longer operating on a live object, any change here will have no effect.
	UpdateStuff();
}
```

The problem is that in `Super::PostEditChangeProperty(Event);` all Blueprint spawned [[Component]]s of the [[Blueprint Instance]] is destroyed and recreated, including the [[Component]] that [[PostEditChangeProperty]] is currently being executed on.
That is, before `Super::PostEditChangeProperty(Event);` `IsValid(this)` will be `true` but after `Super::PostEditChangeProperty(Event);` it will instead be `false`.
Worse, serialization of the [[Component]] happens before `PostEditChangeProperty` is called so we can't even communicate any changes we want to make to the new [[Component]] instance by delaying the call to `Super::PostEditChangeProperty` until after we've made our change.
```cpp
void UMyComponent::PostEditChangeProperty(FPropertyChangedEven& Event)
{
	// Any changes we make to 'this' here will be lost since the serialization
	// has already been done.
	UpdateStuff();

	// Delaying this call doesn't help, since the serialization data was written
	// before the call to this's PostEditChangeProperty.
	Super::PostEditChangeProperty(Event);
}
```

One way to handle this is to delay the `UpdateStuff` call to instead happen in the new instance of the [[Component]], during its initialization [(1)](https://victor-istomin.github.io/c-with-crosses/posts/ue-post-edit-property/).
A drawback of this approach is that [[Component]]s are initialized in some order and [[Component]]s a particular [[Component]] depend on may not have been initialized yet.
This is not a problem when using the `PostEditChange(Chain)?Property` callbacks since at that point the [[Actor]] is known to be fully up and running.
Because of this we may not be able to call our worker function, `UpdateStuff` in this example, immediately.
One way to handle this is to do the initialization in the next tick, when we know that all [[Component]]s in the reconstructed [[Blueprint Actor]] has been created.
If there are dependencies between the initialization work, not just on the the dependee [[Component]]s being created, then I don't know what to do.
Orchestrate the initialization in a separate class / function that initializes all of them deterministically?

```cpp
void UMyComponent::PostInitProperties()
{
	Super::PostInitProperties();

	if (UWorld* World = GetWorld(); IsValid(World) && !IsTemplate())
	{
		World->GetTimerManager().SetTimerForNextTick([this]{ UpdateStuff(); });
	}
}
```

# References

- 1: [_UE 5: Pitfalls of UActorComponent::PostEditChangeProperty_ by C with Crosses @ victor-istomin.github.io 2023](https://victor-istomin.github.io/c-with-crosses/posts/ue-post-edit-property/)
- 2: [_UE 5: Pitfalls of UActorComponent::PostEditChangeProperty_ @ reddit.com 2023](https://www.reddit.com/r/unrealengine/comments/14yzisc/ue_5_pitfalls_of/)
