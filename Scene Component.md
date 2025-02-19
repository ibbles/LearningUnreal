A Scene Component is a type / subclass of [[Actor Component]].
A Scene Component has a placement in the [[Actor]], i.e. a location and a rotation.
The location and rotation can be either relative to the owning [[Actor]] or absolute in the world.
We can set the world location with the Set World Location node.

Scene Components can be attached to each other,
forming a hierarchy.
In this case a Scene Component's transform is not relative to the owning [[Actor]] but to the parent Scene Component in the hierarchy.


# Root Component

Any [[Actor]] that have as least one Scene Component should have a special Scene Component called the Root Component or the Default Scene Root.
This is the Scene Component that defines the location and rotation of the [[Actor]] in the game world.
When you call Get Actor Location then you will actually get the Location of the [[Actor]]'s Root Component.
The rest of the Scene Components in the [[Actor]] are attached in a hierarchy rooted at the Default Scene Root.
The root component should, I think, be named using `USceneComponent::GetDefaultSceneRootVariableName()`.
```cpp
AMyActor::AMyActor()
{
	RootComponent =
		CreateDefaultSubobject<USceneComponent>(USceneComponent::GetDefaultSceneRootVariableName());
}
```

We may want to call `SetRootComponent(RootComponent)` as well, though as of Unreal Engine 5.3 `SetRootComponent` has a condition on the parameter being different from `RootComponent`, which it isn't in this case since we explicitly and directly set `RootComponent` immediately.
Is this setup incorrect?
Is `RootComponent` not a "complete" Root Component since we didn't call `SetRootComponent`, and even if we did `SetRootComponent` would do nothing?


# Scene Component Subclasses

There are many subclasses of Scene Component.
Any Component that needs a position inherits from Scene Component.
For example [[Spotlight]], [[Primitive Component]] or any other Component type that is rendered, and [[Audio Component]].


# References

- [_Breaking Down the Components of Gameplay_ > _Component Design Choices_ by Epic Games, Rob @ dev.epicgames.com 2021](https://dev.epicgames.com/community/learning/courses/mo/unreal-engine-breaking-down-the-components-of-gameplay/aDO/unreal-engine-component-design-choices)
