The Game Mode is part of the [[Gameplay Framework]].
Server based manager [[Actor]] that is associated with a particular level.
Different levels may have different Game Modes, configured from the [[World Settings]] panel.
The default Game Mode is selected in the [[Project Settings]].
The Game Mode is the first [[Actor]] created when the level is loaded and destroyed when the level is unloaded.

The Game Mode is responsible for the overall rules, logic, and structure of a game session.
The data that is run through that rule set is held by the [[Game State]] and [[Player State]].

The Game Mode control the type of some of the [[Actor|Actors]] that are created by when the game starts.
It instantiates:
- [[Pawn]]
- [[HUD]]
- [[Player Controller]]
- [[Game State]]
- [[Player State]]
- [[Spectator]]

When a player joins the game the Game Mode creates a [[Pawn]], [[Player Controller]], and [[Player State]] for that player.

The class to use for each of these is set in the [[Details Panel]] of the Game Mode.

Create a new Game Mode by [[Content Browser]] > right-click > Blueprint Class > Game Mode Base > Select.
Game Modes are often named with the `GM_` prefix.

The Game Mode to use can be set either in the [[Project Settings]] or the [[World Settings]] panel for a particular level.
So either
[[Project Settings]] > Project > Maps & Modes > Default Game Mode
or
Top Menu Bar > Window > [[World Settings]] > Game Mode > Game Mode Override.
The Game Mode set in the [[Project Settings]] is used for all levels that doesn't have a Game Mode Override set.

Example:
- A player's health, owned by the [[Player State]], reaches zero or below.
- The [[Player State]] informs the [[Game Mode]] about this.
- The [[Game Mode]] fires a life-lost event.
- Any logic associated with the life-lost event is run.
- The [[Player State]] updates the [[Game Instance]] about the new game-wide state.
- The [[Game Instance]] restarts the level, loads the latest save, or returns to the title screen.

# References

- [_Begin Play | Gameplay_ by Epic Games, Samuel Bass @ dev.epicgames.com 2022](https://dev.epicgames.com/community/learning/tutorials/l21z/unreal-engine-begin-play-gameplay)
