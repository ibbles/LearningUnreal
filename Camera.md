A Camera define how we look at the scene while playing.
It has a position, orientation, a field of view, and some other camera-related properties,
some of which are [[Post Processing]]-related,
and also a bunch of other properties that control the way the scene is presented.

Exists as both an [[Actor]] and a [[Actor Component]].
The Camera [[Actor]] is a wrapper around a Camera [[Component]].
The Camera Component inherits from [[Scene Component]],
which means that it has a pose and can be attached to another [[Scene Component]].

Camera Actor has a View Target Priority.
When a [[Level]] is loaded it is the View Target Priority that control which Camera the player will view the scene through.
A Camera is only considered if Auto Activate For Player is set to the player in question, often Player 0 in a single-player game.
Using View Target Priority and Auto Activate For Player we can create fixed view points that are independent of the [[Pawn]]'s location.
See also [[Player Camera Manager]].

A Camera Component can be part of many types of Actors.
It is common to make it part of a [[Pawn]].
To get a [[Third-Person View]] it is common to attach the camera to a [[Spring Arm Component]] or a [[Camera Boom]].
To get a first-person view the camera is placed at head-height.

A Camera can run [[Sequencer]] animations for cinematic transitions or first-person animations.

The camera can be accessed by the [[Player Controller]].
The [[Player Controller]] can move the players view from one Camera to another with the Set View Target With Blend function.

The view frustum of all cameras can be visualized in the [[Level Viewport]] with Show > Advanced > Camera Frustums.
Purple lines outline the visible volume for each camera.


# Types Of Cameras

These types describe camera usage and how the gameplay logic has been set up.
It is not different types from a type / class system point of view.
They are conventions may may chose to follow.

## Static / Fixed

Can be a Camera Actor we manually placed in the world.
Does not necessarily look at the [[Pawn]], it can look at anything.
The [[Player Controller]] has no control over this type of camera.

## Dynamic

Liked static / fixed camera the player has no direct control of a dynamic camera.
Unlike static / fixed camera this type of camera may move or orient itself based on gameplay logic.
An example is a camera that has a fixed location but turns to look at the [[Pawn]].
The camera control is based on rules, not player input.

## Active

An active camera is one that the player has direct control over.
It is actively updating its position and orientation based on player input.
May layer other behavior on top of the player input, such as avoiding obstacles and preventing things getting in between the camera and the [[Pawn]].


# References

- [_Begin Play | Gameplay_ by Epic Games, Samuel Bass @ dev.epicgames.com 2022](https://dev.epicgames.com/community/learning/tutorials/l21z/unreal-engine-begin-play-gameplay)
- [_Camera Framework Essentials for Games_ by Epic Games, Rob Brooks @ dev.epicgames.com 2022 UE 5.0](https://dev.epicgames.com/community/learning/courses/RRr/unreal-engine-camera-framework-essentials-for-games/wv7n/unreal-engine-camera-framework-essentials-for-games-overview)
