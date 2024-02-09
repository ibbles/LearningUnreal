Collision is the broad concept of detecting and responding to impacts and overlaps, a.k.a. spatial interactions, between objects in the world.
All collidable objects have an object type that determines how it interacts with the collision channels.
Unreal Engine comes with a set of object types and channels.
For example:
- Pawn
- World Prop

These sets can be extended from [[Project Settings]] > Engine > Collision.

A spatial interaction produces one of three results:
- Block: Penetration is prevented by the physics engine.
- Overlap: 
- Ignore: 

When a spatial interaction is in the Block mode it can be configured to fire a Hit [[Event]], which will let project-specific logic to run in response to that interaction.
The Hit Event callback is given a bunch of information such as which [[Actor|Actors]] and [[Actor Component|Components]] collided, where the collision occurred, and what [[Physics Material|Physics Materials]] the objects have.

It is common to set Pawn - World Prop collisions to Ignore so that the player doesn't get stuck everywhere.

A variant of a collision check is a [[Line Trace]].


# References

- [_Begin Play | Gameplay_ by Epic Games, Samuel Bass @ dev.epicgames.com 2022](https://dev.epicgames.com/community/learning/tutorials/l21z/unreal-engine-begin-play-gameplay)

