A [[Niagara System]] is a the thing that _is_ a particle system in Niagara.
It is a container for various other stuff, such as emitters and .... (Anything else?)
A Niagara System is an [[Asset]] that can be created from and lives in the Content Browser.
Create a new Niagara System by [[Content Browser]]  > right-click > Niagara System > pick one of the templates.
A Niagara System can be placed or spawned in a [[Level]].
Drag the particle system from the [[Content Browser]] to the [[Level Viewport]] to create an instance.

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
	UNiagaraComponent* Particles =
		UNiagaraFunctionLibrary::SpawnSystemAtLocation(
			GetWorld(), ParticleSystem, GetActorLocation());
}
```

An alternative to `SpawnSystemAtLocation`  is `SpawnSystemAttached`,
which makes the spawned particle system follow, i.e. be attached to, a [[Scene Component]] we provide.


# User Parameters

A Niagara System can contain user parameters.
These are values that can be passed in from e.g. a Details panel or set by a [[Blueprint Visual Script]].

Add a new parameter by selecting the blue system node in the [[Niagara Editor]] > User Parameters > click the blue + button to the right.
Each parameter has a type.

Any parameter to a [[Niagara Module]] in the system can be bound to these user parameters.


# Particle Attributes

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