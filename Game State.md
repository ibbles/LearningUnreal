The Game State is part of the [[Gameplay Framework]].
The Game State holds the state of the overall game.
"Everything" that is not part of the [[Player State]] or any particular [[Actor]] or [[Component]].
Data and logic that is relevant to all players.
Like team score or shared mission objectives.
The Game State holds the data that is run through the [[Game Mode]]'s rule set.
The Game State holds the player array which contains the Player State instance for every client.

Example:
- A player's health, owned by the [[Player State]], reaches zero or below.
- The [[Player State]] informs the [[Game Mode]] about this.
- The [[Game Mode]] fires a life-lost event.
- Any logic associated with the life-lost event is run.
- The [[Player State]] updates the [[Game Instance]] about the new game-wide state.
- The [[Game Instance]] restarts the level, loads the latest save, or returns to the title screen.

The Game State is replicated across the server and the clients.

# References

- [_Begin Play | Gameplay_ by Epic Games, Samuel Bass @ dev.epicgames.com 2022](https://dev.epicgames.com/community/learning/tutorials/l21z/unreal-engine-begin-play-gameplay)
