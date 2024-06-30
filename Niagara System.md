A [[Niagara System]] is a the thing that _is_ a particle system in Niagara.
The things that represents a whole effect, such as an explosion or a flame with smoke.
It is a container for various other stuff, such as emitters, parameters, and .... (Anything else?)

# Creating A Niagara System

A Niagara System is an [[Asset]] that can be created from and lives in the [[Content Browser]].
Create a new Niagara System by [[Content Browser]]  > right-click > _Create Basic Asset_ > Niagara System > pick one of the templates.

Niagara Systems are often named with the `NS_` prefix, for Niagara System.

Double-click the Niagara System [[Asset]] to open the [[Niagara Editor]].
See [[Niagara Emitter]], [[Niagara Module]], and [[Niagara Parameter]] to learn how to populate the System with particles and behavior.

# Niagara System Settings

The Niagara System node in the [[Niagara Editor]] is the blue one.
Select it, or one of its modules, to populate the Selection panel with its properties.

## System State

Loop Behavior:
- Once
	- When an instance of the System is spawned in the [[Level]] Emitters are spawned with it and live until all Emitters and / or (Which is it?) and particles have aged out. The the Particle System if empty and destroyed (I think.).
- Infinite
	- The particle system is reset and restarted at regular intervals. Emitter Spawn is run at each restart. The time between resets is controlled with Loop Duration.
- Multiple
	- Like Infinite, but with a limited number of loops.


# Spawning A Niagara System

A Niagara System can be placed or spawned in a [[Level]].
Drag the particle system from the [[Content Browser]] to the [[Level Viewport]] to create an instance.
Systems with Loop Behavior set to Once are usually not placed like this since they would only run once when the level is loaded and the disappear.
It is more common to spawn a System in response to user action or game logic.

## Blueprint

To spawn a particle system at runtime from Blueprint Visual Script use one of
- Spawn System At Location
- Spawn System At Location With Params
- Spawn System Attached
- Spawn System Attached With Params

Set System Template to the [[Niagara System]] [[Asset]] to spawn.

Once the system has been spawned we can set parameters on it.
Use the Set Niagara Variable node to do this.
This node has a text input pin named Variable Name for the name of the variable to set.
The name should include the [[Niagara Namespace]].
This node cannot set variables in the PARTICLES namespace.
And probably many others as well.
It can set USER parameters.
See _Parameters_ below.


# C++

To spawn a particle system at runtime from C++:

`MyActor.h`:
```cpp
#pragma once
#include "GameFramework/Actor.h"
#include "MyActor.generated.h"
class UNiagaraSystem;

UCLASS(Blueprintable)
class MYMODULE_API AMyActor : public AActor
{
	GENERATED_BODY()

public:
	void EmittParticles();

	// A reference to a Niagara System asset, not an in-world instance.
	UPOPERTY(EditAnywhere, Category="Effects")
	UNiagaraSystem* ParticleSystem;
}
```

`UNiagaraSystem` is the type of the Niagara System [[Asset]].
So `ParticleSystem` doesn't point to an actual instance in  the world, but a description of a particle system.
To get an instance in the level we must spawn a new instance of the `UNiagaraSystem`.

`MyActor.cpp`:
```cpp
void AMyActor::EmittParticles()
{
	// You can make the UNiagaraComponent a member variable to
	// interact with it later.
	UNiagaraComponent* Particles =
		UNiagaraFunctionLibrary::SpawnSystemAtLocation(
			GetWorld(), ParticleSystem, GetActorLocation());
}
```

An alternative to `SpawnSystemAtLocation`  is `SpawnSystemAttached`,
which makes the spawned particle system follow, i.e. be attached to, a [[Scene Component]] we provide.


# Parameters

A Niagara System contains data that the [[Niagara Emitter]]s, and their [[Niagara Module]]s, can access.
Through this data [[Niagara Emitter]]s can talk to each other.

## User Parameter

A Niagara System can contain user parameters.
These are values that can be passed in to an instance of the System from e.g. a Details panel or set by a [[Blueprint Visual Script]].
Used for fine-tuning of an effect for a particular use-case or setting.

Add a new parameter by selecting the blue system node in the [[Niagara Editor]] > User Parameters > click the blue + button to the right.
Each parameter has a type, so a list of possible types is opened. Pick one.
Give the parameter a name and a default value in the Selection panel.
User parameters are always in the USER [[Niagara Namespace]].
When an instance of the System is selected in the [[Level Viewport]] we can find the new parameter at [[Details Panel]] > User Parameters.

Any parameter in a [[Niagara Module]] in the system can be bound to these user parameters.
Select the module with the parameter, click the ﹀ button > select Link Inputs > User > USER `variable name`.

## Setting A Parameter From Blueprint Visual Script

Use the Set Niagara Variable node to set a variable on a Niagara System.
Only parameters within the USER [[Niagara Namespace]] can be set.
To set a parameter named "My Parameter", use the name "USER.My Parameter".


# Particle Attribute

A Niagara System contains a collection of particles and each particle carries data in the form of [[Niagara Attribute|Niagara Attributes]].
An Attribute is like a member variable in a struct, and all particles are like instances of that struct.
The Attributes are listed in the Particle Attributes section of the Parameter panel in the [[Niagara Editor]].
Particle Attributes are in the `(PARTICLES)` [[Niagara Namespace]].

Attributes can be either-built in or custom.
Built-in means that come with Niagara itself and can't be removed.
These Attributes are marked with a lock symbol in the Particle Attributes section of the Parameters panel.
Custom Attributes can be added and removed at will.
Add a new Attribute by clicking the `+` button in the Particle Attributes section header in the Parameters panel.
This will present a list of data types, [[Niagara Data Interface|Niagara Data Interfaces]], enums, events, objects, and common (or built-in?) `(PARTICLE)` Attributes such as ID, Lifetime, Scale, and many more.

Particle Attributes can be bound to [[Niagara Module]] input parameters by dragging the Attribute from the Parameters panel to the parameter's value box in the Selection panel when an instance of the Module is selected in a [[Niagara Emitter]].

# References

- [_Unreal Engine 4.26.0 Niagara Growing Trees Tutorial_ by Art Hiteca @ youtube.com 2020](https://youtu.be/DV1cPrYHtYg?t=337)
- [_Begin Play | Niagara_ by Epic Online Learning, Arran Langmead @ dev.epicgames.com/tutorials 2023 UE5.0](https://dev.epicgames.com/community/learning/tutorials/j9YO/unreal-engine-begin-play-niagara)
- [_Intro To Niagara_ by Epic Online Learning, James Hill @ dev.epicgames.com/tutorials 2023 UE5.2](https://dev.epicgames.com/community/learning/tutorials/8B1P/unreal-engine-intro-to-niagara)
