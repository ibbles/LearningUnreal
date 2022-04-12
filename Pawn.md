A Pawn is an [[Actor]] that can be possessed by a [[Controller]].
A commonly used subclasse of Pawn is [[Character]].
A [[Controller]] can be either an [[AI Controller]] or a [[Player Controller]].
Pawns controlled by a [[Player Controller]] is spawed at the [[Player Start]] [[Actor]].

# Control
One way to control a Pawn is to call the Add Movement Input function on it.
This forwards the movement input to the Pawn's Movement Component. (I think.)
