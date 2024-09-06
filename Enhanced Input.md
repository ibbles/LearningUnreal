Enhanced Input is the input system introduced with Unreal Engine 5,
replacing [[Input Bindings]].

The Enhanced Input system consists of two main parts:
- Input Action: A action that the player can take. A thing that happens when the player press a key on the keyboard or clicks the mouse.
- Input Mapping Context: A collection of related Input Actions.

# Input Action

An Input Action represents something that the player can do.
Examples:
- Use
- Pick up
- Walk
- Crouch
- Turn Right
- Cast spell in spell slot 3.

An Input Action is typically associated with some kind of hardware input,
such as a key press, a mouse click, or a gamepad axis.
However, the Input Action itself does not decide which hardware input.
That is controlled by an Input Mapping Context.

An Input Action is a type of [[Asset]].
Create a new Input Action with [[Content Browser]] > right-click > Input > Input Action.
Input Actions typically have a `IA_` prefix.

An Input Action is a type of [[Event]], meaning that [[Actor]]s and [[Component]]s can listen for and react to the Input Action being triggered.
When triggered the Input Action provides a value corresponding to what the user did.
A button event is either true of false depending on if the button was pressed or released.
A gamepad stick or mouse movement can be either a 1D or 2D value representing the angle of the stick or the motion of the mouse.
A VR motion controller would be a 3D value.
In the Input Action this is called its Value Type and the options are:
- Digital (bool)
- Axis1D (float)
- Axis2D (Vector2D)
- Axis3D (Vector)

To listen for the Input Action in an [[Actor]] do [[Event Graph]] > right-click > Input > Enhanced Action Event > your event.


## Consume Input

By default an input is passed to all matching Input Actions ordered by the priority set when the owning Input Mapping Context was added to the Enhanced Input Local Player Subsystem.
If you want to consume, i.e. hide from lower-priority listeners, the input then tick Input Action > Input Consumption >
- Consume Lower Priority Enhanced Input Mappings.
	- To hide the input from [[Actor]]s using Enhanced Input.
- Consumes Action And Axis Mappings.
	- To hide the input from [[Actor]]s using the legacy input system.

You must also tick some of the Trigger Events That Consume Legacy Keys entries.

## /

When creating a new Input Action remember to
- add it to the owning Input Mapping Context.
- add an event for it in the [[Actor]]'s [[Event Graph]].


# Input Mapping Context

An Input Mapping Context is a collection of related Input Actions and rules for triggering them.
Example contexts:
- Controlling a character walking around.
- Controlling a character running around.
- Controlling a vehicle.
- Quick-time event.
- Being in a menu system.
- Being a spectator.

Multiple contexts can be active at the same time.
The contexts have a priority to control what Input Action is triggered if multiple currently active contexts are in conflict.

An Input Mapping Context is an [[Asset]].
Create a new Input Mapping Context with [[Content Browser]]  > right-click > Input > Input Mapping Context.
Input Mapping Contexts typically have a `IMC_` prefix.

The Input Mapping Context contains a list of mappings.
Each mapping is an Input Action and a list of hardware inputs that should trigger that Input Action when the Input Mapping Context is active.
Hardware inputs include keyboard keys, mouse buttons, mouse motion, gamepad axes, etc.
Each hardware input can have a modifier which modifies the value.
This is used, for example, to bind D to Turn Right and S also to Turn Right but with the Negate modifier.
There is also a modifier that implements gamepad and joystick deadzone.

An Input Mapping Context can be activated and deactivated for a particular [[Player Controller]] (Is this true? What about Enhanced Input Local Player Subsystem? Is that a part of the [[Player Controller]]?) by gameplay logic.
Only Input Actions that are part of a currently active Input Mapping Context can be triggered by the player.


# Activating An Input Mapping Context

An Input Mapping Context is activated with the Enhanced Input Local Player Subsystem.
When registering an Input Mapping Context you can provide a priority that is used to resolve conflicting bindings.
(
I don't understand the [documentation](https://docs.unrealengine.com/5.3/en-US/enhanced-input-in-unreal-engine/#inputmappingcontexts) on this.

> If you have multiple contexts mapped to the same Input Action, then, when the Input Action is triggered, the context with the highest priority will be considered and the others ignored.

Do they mean "Input", as in key, button, stick, instead of Input Action?
How can it be a problem to, for example, have both Spacebar and Left Mouse Button bound to Fire?
As opposed to having Spacebar bound to both Jump and Fire.
)


## Activating An Input Mapping Context In Blueprint

In a [[Pawn]]: Get Controller > Cast To Player Controller > Enhanced Input Local Player Subsystem > Add Mapping Context.


## Activating An Input Mapping Context In C++

`AMyPawn.h`:
```cpp
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Pawn.h"

UCLASS(Blueprintable)
class MYMODULE_API AMyPawn : public APawn
{
	GENERATED_BODY();
public:
	// This one point to an Input Mapping Context asset.
	UPROPERTY(EditAnywhere, Category = "Input")
	TSoftObjectPtr<UInputMappingContext> InputMapping;

	void ActivateInputMapping();
}
```

`AMyPawn.cpp`:
```cpp
void AMyPawn::ActivateInputMapping()
{
	// Where did 'Player' come from?
	// Where did 'Priority' come from?
	ULocalPlayer* LocalPlayer = Cast<ULocalPlayer>(Player);
	UEnhancedInputLocalPlayerSubsystem* InputSystem = LocalPlayer->GetSubsystem<UEnhancedInputLocalPlayerSubsystem>();
	InputSystem->AddMappingContext(InputMapping.LoadSynchronous(), Priority);
}
```


# Listening For Input Action Events

An Input Action fire an [[Event]] when triggered.
An [[Actor]] or [[Actor Component]] (Other types as well?) can listen for this event to perform some action when it is triggered.

## Listening For Input Action Events In Blueprint

To listen for a particular Input Action event right-click the [[Event Graph]] and find the Input Action name, the one often starting with `IA_`, under Input > Enhanced Action Event.
The event node that is created contains one or more execution pins, one for each trigger state of the Input Action, and a value pin corresponding to the Value Type of the Input Action.

## Listening For Input Action Events In C++

In a C++ [[Actor]] the listening is setup in `SetupPlayerInputComponent`.
I don't know how to do this in an Actor Component.

`MyActor.h`:
```cpp
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"

UCLASS(Blueprintable)
class MYMODULE_API AMyActor : public AActor
{
	GENERATED_BODY()
public:
	virtual void SetupPlayerInputComponent(UInputComponent* InputComponent);
	void OnMyAction(const FInputActionInstance& Instance);
}
```

`MyActor.cpp`:
```cpp
void AMyActor::SetupPlayerInputComponent(UInputComponent* InputComponent)
{
	UEnhancedInputComponent* Input = Cast<UEnhancedInputComponent>(InputComponent);
	Input->BindAction(??? /*Don't know what this is.*/, ETriggerEvent::Triggered, this, &AMyActor::OnMyAction);
}

void AMyActor::OnMyAction(const FInputActionInstance& Instance)
{
	const bool bDown = Instance.GetValue().Get<bool>();
	if (bDown)
	{
		// Start doing the action.
	}
	else
	{
		// Stop doing the action.
	}
}
```

I don't know what the first parameter to `BindAction` is.
I understand that it is the Input Action we want to listen for, but how do we identify it?
The Input Action may not even exist yet when the C++ code is compiled.
The [documentation](https://docs.unrealengine.com/5.3/en-US/API/Plugins/EnhancedInput/UEnhancedInputComponent/BindAction/) state that we may pass either a `UInputAction` or a `FName`.
Passing a name seems easy enough, assuming it is the name we give the Input Action asset.
But what if there are multiple Input Action assets with the same name in multiple locations in the [[Content Browser]]?
Is the name really a path?
Does it pick the first it finds?
Is it not allowed to have multiple Input Action assets with the same name even if they are in different folders?

I don't know what happens if we pass the wrong template type to `Get<>`.


# Trigger State


# Debugging

[[Console Command]]:
- `showdebug enhancedinput`
- `showdebug devices`
# References

- [_Enhanced Input_ by Epic Games @ docs.unrealengine.com UE5.3](https://docs.unrealengine.com/5.3/en-US/enhanced-input-in-unreal-engine/)
