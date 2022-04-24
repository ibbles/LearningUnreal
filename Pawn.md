A Pawn is an [[Actor]] that can be possessed by a [[Controller]].
A commonly used subclass of Pawn is [[Character]].
A [[Controller]] can be either an [[AI Controller]] or a [[Player Controller]].
Pawns controlled by a [[Player Controller]] is spawned at the [[Player Start]] [[Actor]].
We can check if a particular Pawn is controlled by a [[Player Controller]] with the Is Player Controlled node.

The [[Class Defaults]] contains Pawn > Use Controller Rotation {Pitch, Yaw, Roll}.

# Control
One way to control a Pawn is to call the **Add Movement Input** function on it.
This forwards the movement input to the Pawn's Movement Component. (I think.)
Add Movement Input takes a World Direction that is the direction to move.
We can base the direction either on the Pawn itself or its Controller.
To base it on the Pawn itself, pass in **Get Forward Vector, Get Right Vector**, or similar.
To base it on the Controller, call Get Forward Vector, Get Right Vector, or similar on **Get Controller**.
To constrain the movement to be **on the ground plane** get the Rotator for either the Pawn or the Controller, pass the Z component only to Make Rotator, leaving X and Y at 0.0, and call Get Forward Vector, Get Right Vector, or similar on that.

