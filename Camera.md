A Camera define how we look at the scene while playing.
It has a position, orientation, a field of view, and some other camera-related properties,
some of which are [[Post Processing]].
And also a bunch of other properties that control the way the scene is presented.

A Camera Component can be part of many types of Actors.
It is common to make it part of a [[Pawn]].
To get a [[Third-Person View]] it is common to attach the camera to a [[Spring Arm Component]] or a [[Camera Boom]].
To get a first-person view the camera is placed at head-height.

A Camera can run [[Sequencer]] animations for cinematic transitions or first-person animations.

The camera can be accessed by the [[Player Controller]].
The [[Player Controller]] can move the players view from one Camera to another with the Set View Target With Blend node.

# References

- [_Begin Play | Gameplay_ by Epic Games, Samuel Bass @ dev.epicgames.com 2022](https://dev.epicgames.com/community/learning/tutorials/l21z/unreal-engine-begin-play-gameplay)
