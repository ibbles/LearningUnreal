The Controller is part of the [[Gameplay Framework]].

A Controller controls a [[Pawn]].
Think of them as the brain and the body.
We say that a Controller possesses a [[Pawn]].
The Controller has the Possess member function, that takes a [[Pawn]] parameter.
When possessed, the [[Pawn]] receives commands from the [[Controller]], which the [[Pawn]] carries out.
The Controller drive the behavior of the [[Pawn]],
either based on player input or AI logic.

Controller is the base class for [[Player Controller]] and [[AI Controller]].
The [[Player Controller]] translates player input into commands to the [[Pawn]].
The [[AI Controller]] handles AI logic, path-finding, and such.

The Controller takes control of the [[Pawn]]'s [[Camera]].

It is a design decision which parts of this chain is in the Player Controller and which is in the [[Pawn]].
Anything put into the [[Pawn]] will be specific to that [[Pawn]].
Keeping the two separate allows for more flexibility and reuse.
The same Controller can be designed to handle many types of [[Pawn]]s,
especially if provided configuration options for variations of [[Pawn]]s.
Especially useful for an [[AI Controller]].

# References

- [_Begin Play | Gameplay_ by Epic Games, Samuel Bass @ dev.epicgames.com 2022](https://dev.epicgames.com/community/learning/tutorials/l21z/unreal-engine-begin-play-gameplay)
