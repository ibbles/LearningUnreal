A Pawn is an [[Actor]] that can be possessed by a [[Controller]].
A commonly used subclass of Pawn is [[Character]].
A [[Controller]] can be either an [[AI Controller]] or a [[Player Controller]].
Pawns controlled by a [[Player Controller]] is spawned at the [[Player Start]] [[Actor]].
We can check if a particular Pawn is controlled by a [[Player Controller]] with the Is Player Controlled node.

# Control
One way to control a Pawn is to call the Add Movement Input function on it.
This forwards the movement input to the Pawn's Movement Component. (I think.)
