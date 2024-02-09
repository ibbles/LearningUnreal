The Controller is part of the [[Gameplay Framework]].

A Controller controls a [[Pawn]].
Think of them as the brain and body.
We say that a Controller possesses a [[Pawn]].
When possessed, the [[Pawn]] receives commands from the [[Controller]], which the [[Pawn]] carries out.
The Controller drive the behavior of the [[Pawn]],
either based on player input or AI logic.

Controller is the base class for [[Player Controller]] and [[AI Controller]].
The [[Player Controller]] translates player input into commands to the [[Pawn]].
The [[AI Controller]] handles AI logic, path-finding, and such.


# References

- [_Begin Play | Gameplay_ by Epic Games, Samuel Bass @ dev.epicgames.com 2022](https://dev.epicgames.com/community/learning/tutorials/l21z/unreal-engine-begin-play-gameplay)
