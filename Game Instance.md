The Game Instance is part of the [[Gameplay Framework]].
The Game Instance is created at engine launch and is persistent throughout the entire game session.
This means that it can carry data between level transitions.
It manages things like world-state data, player party composition, music, etc.
Player data you wish to transfer from one level to the next can be read from the [[Player State]] at the end of the old level and stored in the Game Instance until a new [[Player State]] has been created in the new level, at which point the data in the Game Instance is used to initialize the new [[Player State]].
A similar technique can be used to implement a save game system.

The Game Instance is a manager class that doesn't represent anything in the game world,
it is a behind-the-scenes object.

It can be implemented in either C++, Blueprint, or a combination of both.
The Game Instance can be accessed from any Blueprint (I think) with the Get Game Instance node.

Example:
- A player's health, owned by the [[Player State]], reaches zero or below.
- The [[Player State]] informs the [[Game Mode]] about this.
- The [[Game Mode]] fires a life-lost event.
- Any logic associated with the life-lost event is run.
- The [[Player State]] updates the [[Game Instance]] about the new game-wide state.
- The [[Game Instance]] restarts the level, loads the latest save, or returns to the title screen.

The Game Instance is not replicated,
the server and the clients have separate instances.
Use the [[Game State]] and [[Player State]] for real-time gameplay data instead,
they can update the Game Instance when necessary.


# References

- [_Begin Play | Gameplay_ by Epic Games, Samuel Bass @ dev.epicgames.com 2022](https://dev.epicgames.com/community/learning/tutorials/l21z/unreal-engine-begin-play-gameplay)
