The Game Mode control the type of some of the [[Actor|Actors]] that are created by default when the game starts.
It controls
- [[Pawn]]
- [[HUD]]
- [[Player Controller]]
- [[Game State]]
- [[Player State]]
- [[Spectator]]

The class to use for each of these is set in the Details panel of the Game Mode.

Create a new Game Mode by Content Browser > right-click > Blueprint Class > Game Mode Base > Select.
Game Modes are often named with the `GM_` prefix.

The Game Mode to use can be set either in the Project Settings or the World Settings for a particular level.
So either
Project Settings > Project > Maps & Modes > Default Game Mode
or
Top Menu Bar > Window > World Settings > Game Mode > Game Mode Override.
The Game Mode set in the Project Settings is used for all levels that doesn't have a Game Mode Override set.
