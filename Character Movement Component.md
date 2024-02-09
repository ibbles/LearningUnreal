A Component that the [[Character]] class has.
Provides basic walking, running, jumping, crouching, flying, and swimming modes.
Each with its own set of options.
it is possible to add your own modes.
The settings in the [[Details Panel]] > Movement Capabilities control which of these modes a particular [[Character]] type can use.

The Character Movement Component configures how a [[Character]] interacts with [[Physics Objects]].
This is important since by default a [[Pawn]] is a kinematic entity from the physics simulation's point of view.

Has a bunch of AI navigation settings.

# Rotation

Has [[Details Panel]] > Character Movement (Rotation Settings) > Use Controller Desired Rotation.
Causes the [[Character]] to be turned to face the direction requested by the [[Controller]].

Has [[Details Panel]] > Character Movement (Rotation Settings) > Orient Rotation To Movement.
Makes the [[Character]] turn towards the direction it is moving.
When using this, also disable [[Character]] > [[Details Panel]] > Pawn > Use Controller Rotation Yaw.

This may also cause a [[Camera]] attached to the [[Character]] to turn in the same direction.
If that is not desired then the Camera's, or a parent Scene Component in the hierarchy such as a Sprint Arm's, Rotation to be world/absolute instead of relative.


# References

- [_Begin Play | Gameplay_ by Epic Games, Samuel Bass @ dev.epicgames.com 2022](https://dev.epicgames.com/community/learning/tutorials/l21z/unreal-engine-begin-play-gameplay)
