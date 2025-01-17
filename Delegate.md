A Delegate is a container for one or more functions to be called at some future time.
Such a function is called a "callback".
A Delegate is a mechanism to build event-driven systems.
A Delegate makes it possible for one part of the system to be notified about events in another part of the system.
Triggering a delegate causes the registered callback functions to be called.
Each delegate has a signature that all registered callbacks must match,
i.e. the callback must have the correct return type and parameter list.
When triggering a delegate we must provide arguments that match the types of the parameter list.
The arguments provided when the delegate is triggered are forwarded to the callback function.

In [[Blueprint Class]] this concept is instead called [[Event Dispatcher]].

Example use-cases:
- Notifying AI that a door has become unlocked.
- Notifying a GUI that the score has changed.
- Notifying game logic when the user interacts with the [[User Interface]].

It is the thing that knows that a change has happened that owns the Delegate.
It is the things that want to know when a change has happened that registers a callback function with the Delegate.
A Delegate is a type and we can have member variables of that type.
For example, a Door [[Actor]] may have a Delegate member variable named On Door Unlocked.
Delegate names often start with "On".
The Delegate type has member functions for registering a callback function.
Different types of Delegate types have different names for the callback-registering member function.
Bind for uni-cast Delegates and Add for multi-cast Delegates.

A Delegate may be static or dynamic, and it may be uni-cast or multi-cast.
- Static or dynamic.
	- If the delegate calls the callback through a function pointer or a function named resolved dynamically at runtime (call time?).
	- Only dynamic delegates can be used from [[Blueprint Visual Script]].
- Uni-cast or multi-cast.
	- If the delegate is limited to a single callback, or can have many.

Delegate types are declared with the `DECLARE_DELEGATE` family of macros.
The macro names follow this pattern:
`DECLARE(_DYNAMIC)?(_MULTICAST)?_DELEGATE(_RetVal)?(_(One|Two|Three|...)Param(s)?)?`

There are four steps involved:
- Declaring the delegate type.
- Declaring a delegate variable, often a class member.
- Registering one more more callback function.
- Triggering the delegate to call the registered function(s).


# Creating A Delegate

Let's create a uni-cast static delegate, which is the simplest case.
Types that include a function signature can be cumbersome to type in C++ so Unreal provides helper macros.
Declare a new delegate type, not variable, with no parameters with the `DECLARE_DELGATE` macro.
The single parameter to `DECLARE_DELEGATE` is the name of the delegate type to create.
The type name given to the `DECLARE_DELEGATE` macro can later be used as the type for a member variable.

If you need to pass a **parameter** to the callback then use the `DECLARE_DELEGATE_OneParam` macro instead.
This macro has one parameter more than `DECLARE_DELEGATE` that goes after the delegate type name and this parameter is the type of the callback parameter.
There is also `DECLARE_DELEGATE_TwoParams`, `DECLARE_DELEGATE_ThreeParams` and so on for additional parameters.
Dynamic delegates differ from static delegates in that the `DECLARE_DYNAMIC_DELEGATE` macro takes two parameters per callback parameter,
the first in each pair the parameter's type and the second is the parameter's name.

For uni-cast delegates, if the callback should have a non-void **return type** then that type should go before the delegate type name.
Also include `_RetVal` in the macro name.
Multi-cast delegates does not support return types, but see _Multicast Delegate_ > _Return Value_ below for a work-around.

The parameter or return types cannot include a comma because the C++ preprocessor doesn't understand template syntax.
Use a using directive to create a comma-less type alias and use the alias instead.

A few delegate type declarations and the signature supported callback functions:
```cpp
DECLARE_DELEGATE(FMyDelegate); // void()
DECLARE_DELEGATE_OneParam(FMyDelegate, int); // void(int)
DECLARE_DELEGATE_RetVal(bool, FMyDelegate); // bool()
DECLARE_DELEGATE_RetVal_OneParam(bool, FMyDelegate, int); // bool(int)
DECLARE_DELEGATE_TwoParams(FMyDelegate, int, float); // void(int, float)
DECLARE_DELEGATE_RetVal_TwoParams(bool, FMyDelegate, int, float); // bool(int, float)

using MyMap = TMap<int, float>;
DECLARE_DELEGATE_OneParam(FMyDelegate, MyMap&); // void(TMap<int, float>&)
```


Delegate declarations, i.e. the macros above, can be decorated with the `UDELEGATE` macro.
It is similar to the [[UFUNCTION]] macro and supports the same set of [[Function Specifier]]s.

# Triggering A Delegate

Different types of delegates are triggered in different ways.
A uni-cast static delegate can be triggered with `ExecuteIfBound`.
Pass arguments to `ExecuteIfBound` matching the parameter list in the `DECLARE_DELEGATE` macro used to declare the delegate's type.
The types must match exactly, not implicit conversions will be performed.
Multi-cast delegates are triggered with Broadcast.


Example creating a triggering a static uni-cast delegate:
`Door.h`
```cpp
#pragma once
#include "GameFramework/Actor.h"
#include "Door.generated.h"
UCLASS()
class MYMODULE_API ADoor : public AActor
{
	GENERATED_BODY()
public:
	DECLARE_DELEGATE(FUnlocked);
	DECLARE_DELEGATE_OneParam(FOpened, float);

	void Unlock();
	void Open(float Angle);

	FUnlocked OnUnlocked;
	FOpened OnOpened;
};
```

Since this is a static, not dynamic, delegate it is not Blueprint-compatible so we don't add the `UPROPERTY(BlueprintAssignable)` decorator.
Static delegates can't be serialized, so after a save / load cycle the callbacks need to be re-registered by the subscribers.
Dynamic delegates can be serialized.
See _Serialization_ below.

`Door.cpp`:
```cpp
void ADoor::Unlock()
{
	// Code to handle unlocking goes here.

	OnOpened.ExecuteIfBound();
}

void ADoor::Open(const float Angle)
{
	// Code to swing the door open to the given angle goes here.

	OnOpened.ExecuteIfBound(Angle);
}
```


# Binding To A Delegate

A uni-cast static delegate will by default do nothing when `ExecuteIfBound` is called.
To actually call a callback we must register the callback function with the delegate.
For a uni-cast static delegate this is called to bind a function to it.
Unreal Engine supports a bunch of categories of callbacks, each with different ownership semantics.
- Global function, `BindStatic`.
	- Binds a global, or free, function, or a static member function, by function pointer to the delegate.
	- Such functions are always available (assuming you don't unload shared libraries) so no ownership rules are required.
- Non-static member function in a [[UObject]], `BindUObject`.
	- Uses the Unreal Engine garbage collection system, i.e. a weak reference, to determine if the object is still alive or not.
- [[UFunction]] in a [[UObject]], `BindUFunction`
	- Not sure how this differs from `BindUObject`, other than the fact that the function must be a [[UFUNCTION]].
	- Use the `GET_FUNCTION_NAME_CHECKED(Class, Name)` macro to get a function name.
- Non-static member function in an object owned by a [[Shared Pointer]], `BindSP`.
	- The delegate keeps a [[Weak Pointer]] to the object and will only call the callback if the object is still alive.
- Non-static member function in a non-owned object, `BindRaw`.
	- Use as a last resort. No ownership help from the delegate itself. You must ensure that the object is still alive when the delegate is triggered.
	- Often used for [[Slate]] callbacks.
- Lambda function, `BindLambda`.
	- The lambda function is copied, (or moved, not sure) into the delegate.
	- The lambda itself is always safe to call, but if it has captured and pointers or references then you must ensure that the objects the pointers point to or references references are still valid when the delegate is triggered.
- Lambda function with [[UObject]], `BindWeakLambda`.
	- A way to let the delegate know about a dependency on some [[UObject]] from the lambda function.
	- The delegate will keep a weak reference to the given [[UObject]] and only call the lambda function if the [[UObject]] is still alive.

Do not bind to a delegate in a constructor, bind them in [[Begin Play]] instead.
Binding in the constructor causes the binding to be done for the [[Class Default Object]], which may cause unforeseen problems.


# Multi-Cast Delegate

A Multi-cast Delegate is one that can have multiple listeners.
All Multi-cast Delegates have a signature returning `void`.
This is because the Delegate cannot collect all the return values from all the callbacks,
so for simplicity return values aren't allowed.

A Delegate that is not multi-cast is called a uni-cast delegate.

Multi-cast delegate types are declared with the `DECLARE_MULTICAST_DELEGATE` macro, or any of the `_(One|Two|...)Param(s)?` variants.
A multi-cast delegate can have more than one callback registered at the time.
When the multi-cast delegate is triggered all registered callbacks will be called in sequence.
A multi-cast delegate is triggered with the `Broadcast` member function.

For example, a door may contain a delegate named On Unlocked that is triggered when the door is unlocked.
Anything that want to pass through a locked door would register a callback with the On Unlocked Delegate to be notified when the door is no longer locked.
To signal that we may not be the only thing registered with the delegate the register functions are named `Add...` instead of `Bind...`.
The `...` are the same `Static`, `UObject`, `SP`, `Raw`, `Lambda`, and `WeakLambda` as with the `Bind` functions in uni-cast delegates.

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
	OnUnlocked.Broadcast(this);
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
}

void AMyActor::DoorUnlocked(ADoor* Door)
{
	// The door is now unlocked and we can proceed.
}
```


## Return Value From A Blueprint Delegate

See [[Return Values In Blueprint Implementable Dynamic Delegate]].


# Dynamic Delegate

A dynamic Delegate is one that ties in with the [[Reflection]] system.
A dynamic Delegate can only call functions decorated with [[UFUNCTION]].
The function to call is identified by name, which makes it possible to register a [[Blueprint Function]].

A non-dynamic Delegate identify the function to call with a function pointer instead of a name.
That makes it possible to bind to any C++ function,
but makes it impossible to bind to a [[Blueprint Function]].

The macro to declare a Dynamic Delegate has `DYNAMIC` in its name,
but works much the same as static delegates.
If you want both dynamic and multi-cast then `DYNAMIC` goes before `MULTICAST`:
`DECLARE_DYNAMIC_MULTICAST_DELEGATE(_(One|Two|Three|...)Param(s)?)?`.
One difference with dynamic delegates compared to static is that parameters must be given not only a type but also a name.
`DECLARE_DYNAMIC_DELEGATE_TwoParams(FMyDelegate, float, FirstParam, int32, SecondParam);`

It is possible to pass a string instead of a function pointer to `BindDynamic`,
but I don't know what format the string should be, how function overloading is handled / resolved.
See the `GET_FUNCTION_NAME_CHECKED(Class, Name)` macro.

Incomplete example:
```cpp
DECLARE_DYNAMIC_DELEGATE(FMyDelegate);
FMyDelegate MyDelegate;
MyDelegate.BindDynamic(this, &ThisClass::MyFunction);
MyDelegate.ExecuteIfBound();

DECLARE_DYNAMIC_DELEGATE_OneParam(FMyOtherDelegate, float, MyFloat);
FMyOtherDelegate MyOtherDelegate;
MyOtherDelegate.BindDynamic(this, &ThisClass::MyOtherFunction);
MyOtherDelegate.ExecuteInfBound(SomeFloat);
```

The door example again, but this time with a delegate that can be listened on in a [[Blueprint Class]].
To expose a dynamic delegate to Blueprints it must be both dynamic and multi-cast,
and it must be decorated with the Blueprint Assignable [[Property Specifier]].

`Door.h`:
```cpp
#pragma once
#include "GameFramework/Actor.h"
#include "Door.generated.h"
UCLASS(BlueprintType)
class MYMODULE_API ADoor : public AActor
{
	GENERATED_BODY()
public:
	void DoorOpened(float Angle);

	DECLARE_DYNAMIC_MULTICAST_DELEGATE_OneParam(FOpened, float, Angle);

	UPROPERTY(BlueprintAssignable, Category = "Door")
	FOpened OnOpened;
};
```

`Door.cpp`:
```cpp
#include "Door.h"
void ADoor::DoorOpened(const float Angle)
{
	// Code to open the door to the given angle goes here.

	OnOpened.Broadcast(Angle);
}
```


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
	void ClosedDoorEncountered(ADoor* Door);

	void DoorOpened(float Angle);
};
```

`MyActor.cpp`:
```cpp
void AMyActor::ClosedDoorEncountered(ADoor* Door)
{
	// We closed a locked door and cannot proceed.
	// Register a callback with the door so that we are
	// notified when it is opened.
	Door->OnOpened.AddUniqueDynamic(this, &AMyActor::DoorOpened);
}

void AMyActor::DoorOpened(float Angle)
{
	// Check if the door is opened enough for us to proceed.
}
```

How to have a [[Blueprint Event]] be executed when the door is opened depends on which [[Blueprint Class]] we are in.
If the [[Blueprint Class]] inherits from the C++ class having the delegate member then it shows up in the Events list in the [[Details Panel]].
Click the + button and a [[Blueprint Event]] node is created in the [[Event Graph]] and automatically connected to a Bind Event Node for the delegate.
If you have a reference to an instance of the C++ class then you can drag off of that reference and find the name of the delegate in the list that appears.
Select the delegate and a new list opens, select either Bind or Assign from this new list.
This will create a Bind Event node for the delegate.

(
TODO Read about the `IsBindableEvent` [[Meta Property Specifier]].
See [_Is Bindable Event_ by Ben UI @ benui.ca](https://benui.ca/unreal/uproperty/#isbindableevent).
)

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
- `(Add|Bind)UFunction`
	- Use the `GET_FUNCTION_NAME_CHECKED(Class, Name)` macro to get a function name.
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


# Payload

We can embed extra parameters when adding our callback to a delegate.
The extra parameters are called the callback's payload.
The payload data will be added after the regular delegate parameters that all callbacks are passed.
Only static, i.e. non-dynamic, delegates support payload data.

Example using a single `int` payload.
```cpp
// Doesn't matter who or what owns the delegate.
// What's important for this example is the signature.
DECLARE_DELEGATE_OneParam(FMyDelegate, float); // void(int)
FMyDelegate MyDelegate;

UMyObject::UMyObject()
{
	MyDelegate.AddUObject(this, &UMyObject::Callback, 1);
	MyDelegate.AddUObject(this, &UMyObject::Callback, 2);
}

void UMyObject::Callback(float RegularParameter, int PayloadParameter)
{
	switch (PayLoad)
	{
		case 1:
			// This is the first callback.
			break;
		case 2:
			// This is the second callback.
			break;
	}
}
```

# Serialization

Only dynamic delegates support serialization.
So if you need to save and later restore the list of callbacks, and you can't re-register on load because you know know which callback should be registered to which delegate, then you must use dynamic delegates.

# References

- [_Delegates_ by Epic Games @ docs.unrealengine.com UE 5.3](https://docs.unrealengine.com/5.3/en-US/delegates-and-lamba-functions-in-unreal-engine/)
- [_Dynamic Delegates_ by Epic Games @ docs.unrealengine.com UE 5.3](https://docs.unrealengine.com/5.3/en-US/dynamic-delegates-in-unreal-engine/)
- [_Multi-cast Delegates_ by Epic Games @ docs.unrealengine.com UE 5.3](https://docs.unrealengine.com/5.3/en-US/dynamic-delegates-in-unreal-engine/)
- [_Intro to Delegates in C++_ by ben @ benui.ca 2022](https://benui.ca/unreal/delegates-intro/)
- [_Advanced Delegates in C++_ by ben @ benui.ca 2022](https://benui.ca/unreal/delegates-advanced/)
- [_Can’t bind this delegate?_  @ forums.unrealengine.com 2023](https://forums.unrealengine.com/t/cant-bind-this-delegate/775534)
- [_Delegates in C++_ @ unrealcode.net 2020](https://www.unrealcode.net/content/Delegates.html)
- [_Unreal Engine C++: Event, Dispatch, Delegates etc._ by Rodolphe Vaillant @ radolphe-vaillant.fr 2020](http://rodolphe-vaillant.fr/entry/119/unreal-engine-c-event-dispatch-delegates-etc)
- [_C++ For Unreal Engine (Part 2) | Learn C++ For Unreal Engine | C++ Tutorial For Unreal Engine_ by Nerd's Lesson, A.T. Chamillard @ youtube.com, 2022, UE4.27](https://youtu.be/IYJwU-rB2jA?t=24841)
