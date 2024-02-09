The Pawn is part of the [[Gameplay Framework]].
The Pawn is part of the Pawn, [[Player Controller]], [[Player State]] trio created for each player.

A Pawn is the physical representation of a player or AI within the game world.
A Pawn has a tight relationship with a [[Controller]], which controls the Pawn.
Think of them as the body and brain.

They are used to create player and NPC characters, and many other creatures, vehicles, monsters, and other entities within the game world.

A Pawn is an [[Actor]] that can be possessed by a [[Controller]].
There are several subclasses of [[Pawn]], and a project can add its own subclasses.
A commonly used subclass of Pawn is [[Character]].

A [[Controller]] can be either an [[AI Controller]] or a [[Player Controller]].
Pawns controlled by a [[Player Controller]] is spawned at the [[Player Start]] [[Actor]].
We can check if a particular Pawn is controlled by a [[Player Controller]] with the Is Player Controlled node.

A Pawn has a scene transform determining the Pawn's location, rotation, and scale.
A Pawn has three default [[Component|Component]]:
- Collision
- Mesh
- Movement

The [[Class Defaults]] contains Pawn > Use Controller Rotation {Pitch, Yaw, Roll}.

# Control
One way to control a Pawn is to call the **Add Movement Input** function on it.
Or to one of the **Add Controller {Yaw, Pitch, Roll} Input** functions.
This forwards the movement input to the Pawn's Movement Component. (I think.)

Add **Movement Input** takes a World Direction that is the direction to move.
We can base the direction either on the Pawn itself or its Controller.
To base it on the Pawn itself, pass in **Get Forward Vector, Get Right Vector**, or similar.
To base it on the Controller, call Get Forward Vector, Get Right Vector, or similar on **Get Controller**.
To constrain the movement to be **on the ground plane** get the Rotator for either the Pawn or the Controller, pass the Z component only to Make Rotator, leaving X and Y at 0.0, and call Get Forward Vector, Get Right Vector, or similar on that.

Pawns are kinetic, which means that they appear to have infinite mass in physics interactions with simulated [[Actor|Actors]].
The Pawn will move as the game logic and animation system says that it should move,
and any physics object in the way will simply have to move.
If you need any other behavior then extra logic is required.
The Hit Event described in [[Collision]] may be useful.
A possible response is to prevent or slow the Pawn movement or animation.

# References

- [_Begin Play | Gameplay_ by Epic Games, Samuel Bass @ dev.epicgames.com 2022](https://dev.epicgames.com/community/learning/tutorials/l21z/unreal-engine-begin-play-gameplay)
