There is a type `FDelegate` that is related to, but not exactly the same as, the concept discussed  here.
A Delegate holds one or more `FDelegate` instances that is created by the `(Add|Bind)` functions described below.

A Delegate is a container for one or more functions to be called at some future time.
Such a function is called a "callback".
A Delegate is a mechanism to build event-driven systems.
A Delegate makes it possible for one part of the system to be notified about events in another part of the system.
Triggering a delegate causes the registered callback functions to be called.
Each delegate has a signature that all callbacks must match,
i.e. the callback must have the correct return type and parameter list.

# Example

For example, a door may contain a delegate named On Unlocked that is triggered when the door is unlocked.
Anything that want to pass through a locked door would register a callback with the On Unlocked Delegate to be notified when the door is no longer locked.

`Door.h`
```cpp
#pragma once
#include "GameFramework/Actor.h"
#include "Door.generated.h"

DECLARE_MULTICAST_DELEGATE_OneParam(FOnUnlocked, ADoor*);

UCLASS()
class MYMODULE_API ADoor : public AActor
{
	GENERATED_BODY()
public:
	void Unlock();

	FOnUnlocked OnUnlocked;
};
```

`Door.cpp`:
```cpp
void ADoor::Unlock()
{
	// Whatever logic needed to unlock the door goes here.

	// Trigger the delegating, which causes the registered callbacks
	// to be called.
	OnUnlocked.Broadcast();
}
```

Example of how to bind to a Delegate from some other class:
`MyActor.h`:
```cpp
#pragma once
#include "GameFramework/Actor.h"
#include "MyActor.generated.h"
UCLASS()
class MYMODULE_API AMyActor : public AActor
{
	GENERATED_BODY()
public:
	// Called when the Actor walks up to a locked door.
	void LockedDoorEncountered(ADoor* Door);

	void DoorUnlocked(ADoor* Door);
};
```
`MyActor.cpp`:
```cpp
void AMyActor::LockedDoorEncountered(ADoor* Door)
{
	// We encoutered a locked door and cannot proceed.
	// Register a callback with the door so that we are
	// notified when it is unlocked.
	Door->OnUnlocked.AddUObject(this, &AMyActor::DoorUnlocked);
	// There are other Add-variants. In particular AddSP which
	// seems similar to AddUObject. When should I use which?
}

void AMyActor::DoorUnlocked(ADoor* Door)
{
	// The door is now unlocked and we can proceed.
}
```


# Types of Delegates

A Delegate may be dynamic or not, and it may be multi-cast or not.

## Dynamic Delegate

A Dynamic Delegate is one that ties in with the [[Reflection]] system.
A Dynamic Delegate can only call functions decorated with [[UFUNCTION]].
The function to call is identified by name, which makes it possible to register a [[Blueprint Function]].

A non-Dynamic Delegate identify the function to call with a function pointer instead of a name.
That makes it possible to bind to any C++ function,
but makes it impossible to bind to [[Blueprint Function]].


## Multi-Cast Delegate

A Multi-cast Delegate is one that can have multiple listeners.
All Multi-cast Delegates have a signature returning `void`.
This is because the Delegate cannot collect all the return values from all the callbacks,
so for simplicity return values aren't allowed.

A Delegate that is not multi-cast is called a Uni-Cast Delegate.


# Ways To Register A Function With A Delegate

There are many Delegate member functions used for registering a callback with a Delegate.
Which are available depend on the type of Delegate.
Which you should use depend on the type of function you want to register.
Uni-Cast Delegates have register functions that start with `Bind`.
Multi-Cast Delegates have register function that start with `Add`.

- `AddUFunction`
	- Used when registering a function marked with [[UFUNCTION]].
	- (Is this only for Dynamic Delegate?)
	- (Not described at [_Dynamic Delegates_ by Epic Games @ docs.unrealengine.com UE 5.3](https://docs.unrealengine.com/5.3/en-US/dynamic-delegates-in-unreal-engine/).)
- `(Add|Bind)UObject`
	- Used when registering a member function in a [[UObject]] by function pointer.
	- Cannot be used with a Dynamic Delegate.
	- The Delegate keeps a weak pointer to the [[UObject]] so that it can avoid calling the callback if the [[UObject]] has been destroyed.
- `(Add|Bind)SP`
	- Used when registering a member function in an object that is kept alive by [[Shared Pointer]], i.e. not a [[Garbage Collection|garbage collected]] [[UObject]] 
- `(Add|Bind)Raw`
	- Used when registering a member function in something that doesn't  have any ownership tracking.
	- The Delegate cannot know when the object is destroyed so it is you responsibility to ensure that the callback is removed when the object is destroyed.
- `AddStatic`
	- Used when registering a free / global / non-member function.
	- Cannot be used with Dynamic Delegate.
- `(Add|Bind)Lambda`
	- Used to register a lambda function, or any other functor object.
	- [_Multi-cast Delegates_ by Epic Games @ docs.unrealengine.com UE 5.3](https://docs.unrealengine.com/5.3/en-US/dynamic-delegates-in-unreal-engine/) doesn't list this variant, but the [API Reference for TMulticastDelegate](https://docs.unrealengine.com/5.3/en-US/API/Runtime/Core/Delegates/TMulticastDelegate_void_ParamTyp-/) does.
	- There is family of variants, `(Add|Bind)(SP|Weak)Lambda`, that allows you to specify an object, either [[Shared Pointer]]-owned or [[UObject]], whose lifetime is used to determine if the lambda is safe to call or not.
	- The documentation doesn't explicitly state that `Weak` means `UObject`, but I assume so. For now.


# References

- [_Delegates_ by Epic Games @ docs.unrealengine.com UE 5.3](https://docs.unrealengine.com/5.3/en-US/delegates-and-lamba-functions-in-unreal-engine/)
- [_Dynamic Delegates_ by Epic Games @ docs.unrealengine.com UE 5.3](https://docs.unrealengine.com/5.3/en-US/dynamic-delegates-in-unreal-engine/)
- [_Multi-cast Delegates_ by Epic Games @ docs.unrealengine.com UE 5.3](https://docs.unrealengine.com/5.3/en-US/dynamic-delegates-in-unreal-engine/)
- [_Intro to Delegates in C++_ by ben @ benui.ca 2022](https://benui.ca/unreal/delegates-intro/)
- [_Advanced Delegates in C++_ by ben @ benui.ca 2022](https://benui.ca/unreal/delegates-advanced/)
- [_Can’t bind this delegate?_  @ forums.unrealengine.com 2023](https://forums.unrealengine.com/t/cant-bind-this-delegate/775534)
