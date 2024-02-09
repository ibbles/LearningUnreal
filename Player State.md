The Player State is part of the [[Gameplay Framework]].
The Player State is part of the Pawn, [[Player Controller]], [[Player State]] trio created for each player.
The Player State manages state related to a specific human player.
For example health, ammo count, and inventory.
The Player State holds the data that is run through the [[Game Mode]]'s rule set.
The Game State is replicated across the server and the clients.
A new instance of the class is created whenever a new player joins the game.

Example:
- A player's health, owned by the [[Player State]], reaches zero or below.
- The [[Player State]] informs the [[Game Mode]] about this.
- The [[Game Mode]] fires a life-lost event.
- Any logic associated with the life-lost event is run.
- The [[Player State]] updates the [[Game Instance]] about the new game-wide state.
- The [[Game Instance]] restarts the level, loads the latest save, or returns to the title screen.


# References

- [_Begin Play | Gameplay_ by Epic Games, Samuel Bass @ dev.epicgames.com 2022](https://dev.epicgames.com/community/learning/tutorials/l21z/unreal-engine-begin-play-gameplay)
