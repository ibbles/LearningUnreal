A Character is a type/subclass of [[Pawn]] designed for bipedal animated models.
The Character is the player's or AI's avatar, the body that is running around the level.
This is separate from the brain or mind, which is represented by a [[Controller]], either an [[AI Controller]] or a [[Player Controller]].

Create a new Character class by [[Content Browser]] > right-click > Blueprint Class > Character > click Select.

A Character contains
- A Capsule Component for collision detection.
- An Arrow Component showing the forward direction.
  -  This is in editor-only Component, it is not shown in-game.
- A [[Skeletal Mesh Component]].
- A [[Character Movement Component]].

The Capsule is typically sized to match the size of the Skeletal Mesh.

The Character controlled by the current player, if any, can be fetched with the Get Player Character function.

The [[Pawn]] type, such as Character, created for the player is controlled by the [[Game Mode]].

A Character intended to be controlled by a [[Player Controller]] often contain a [[Camera]].


# References

- [_Begin Play | Gameplay_ by Epic Games, Samuel Bass @ dev.epicgames.com 2022](https://dev.epicgames.com/community/learning/tutorials/l21z/unreal-engine-begin-play-gameplay)
