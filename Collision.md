Collision is the broad concept of detecting and responding to impacts and overlaps, a.k.a. spatial interactions, between objects in the world.
All collidable objects have an object type that determines how it interacts with the collision channels.
Unreal Engine comes with a set of object types and channels.
For example:
- Pawn
- World Prop

These sets can be extended from [[Project Settings]] > Engine > Collision.

A spatial interaction produces one of three results:
- Block: Penetration is prevented by the physics engine.
- Overlap: Overlap [[Event]]s will fire, but the physics engine will not be notified about the interaction.
- Ignore: The two objects aren't aware of each other at all.

When a spatial interaction is in the Block mode it can be configured to fire a Hit [[Event]], which will let project-specific logic to run in response to that interaction.
The Hit Event callback is given a bunch of information such as which [[Actor|Actors]] and [[Actor Component|Components]] collided, where the collision occurred, and what [[Physics Material|Physics Materials]] the objects have.

It is common to set Pawn - World Prop collisions to Ignore so that the player doesn't get stuck everywhere.

A variant of a collision check is a [[Line Trace]].

Instead of listening for overlap events we can also call the Get Overlapping Actors member function on [[Primitive Component]].

# Overlap Callback

Anything that inherits from [[Primitive Component|`UPrimitiveComponent`]], which is most things that can have a collision shape, has a pair of [[Event|Events]] that can be listened on.
The `OnComponentBeginOverlap` [[Event]] is triggered when the [[Primitive Component]] comes into contact with another [[Primitive Component]].
The `OnComponentEndOverlap` [[Event]] is triggered when such an overlap ends, i.e. the [[Primitive Component]]s have moved apart.
The collision settings may influence whether these events are triggered or not.

To set up a callback for a particular member [[Primitive Component]] in an [[Actor]] subclass:
```cpp
#pragme once
#include "GameFramework/Actor.h"

class UMyPrimitiveComponent;

UCLASS()
class MYMODULE_API AMyActor : public AActor
{
	GENERATED_BODY();
public;
	UMyPrimitiveComponent* MyPrimitiveComponent;
private:
	void OnOverlapBegin(
		UPrimitiveComponent* ThisComponent,
		AActor* OtherActor, UPrimitiveComponent* OtherComponent,
		int32 OtherBodyIndex, bool bFromSweep, const FHitResult& SweepResult);

	void OnOverlapEnd(
		UPrimitiveComponent* ThisComponent,
		AActor* OtherActor, UPrimitiveComponent* OtherComponent,
		int32 OtherbodyIndex);
};



AMyActor::AMyActor()
{
	MyPrimitiveComponent = CreateDefaultSubobject<UMyPrimitiveComponent>(TEXT("My Primitive Component"));
	MyPrimitiveComponent->OnComponentBeginOverlap.AddDynamic(this, &AMyActor::OnOverlapBegin);
	MyPrimitiveComponent->OnComponentEndOverlap.AddDynamic(this, &AMyActor::OnOverlapEnd);
}
```

# References

- [_Begin Play | Gameplay_ by Epic Games, Samuel Bass @ dev.epicgames.com 2022](https://dev.epicgames.com/community/learning/tutorials/l21z/unreal-engine-begin-play-gameplay)
- [_Breaking Down the Components of Gameplay_ > _Component Design Choices_ by Epic Games, Rob @ dev.epicgames.com 2021](https://dev.epicgames.com/community/learning/courses/mo/unreal-engine-breaking-down-the-components-of-gameplay/aDO/unreal-engine-component-design-choices)
- [_Becoming an Environment Artist in Unreal Engine_ > _Collison Setup_ by Epic Online Learning @ dev.epicgames.com/courses 2022 UE4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/jMJ/unreal-engine-collison-setup)
