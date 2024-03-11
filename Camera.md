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


# Switching Between Cameras

Each [[Player Controller]] has a View Target.
A View Target is the Camera, or an [[Actor]] that contains a Camera Component, that the player associated with the [[Player Controller]] is viewing the game through.
Use the Set View Target( With Blend)? function to make a [[Player Controller]] switch from one Camera to another.
Pass the [[Pawn]] as the View Target to return to the default camera view.

So you can pass in any [[Pawn]], for example, and the designer of the [[Pawn]] has already decided what Camera should be used when the [[Pawn]] becomes the View Target.
Possessing a [[Pawn]] with a [[Player Controller]] will make that [[Pawn]] the View Target for that [[Player Controller]].
An exception to this rule is a Begin Play if there is a Camera in the level that has [[Details Panel]] > Auto Activate For Player set to the current player.
This setting overrides the possess-means-set-View-Target rule.

If there is no Camera in the View Target then the scene will be rendered with a default camera placed at the View Target [[Actor]]'s origin.

Example in C++:
```cpp
#include "Kismet/GameplayStatics.h"

void UMyObject::SwitchToExternalCamera(AActor* ActorWithCamera)
{
	APlayerController* PlayerController =
		UGameplayStatics::GetPlayerController(this, 0);
	const float BlendTime {1.0};
	PlayerController->SetviewTargetWithBlend(ActorWithCamera, BlendTime);
}

void UMyObject::SwitchToPawnCamera()
{
	APlayerController* PlayerController =
		UGameplayStatics::GetPlayerController(this, 0);
	APawn* Pawn = PlayerController->GetPawn();
	const float BlendTime {1.0};
	PlayerController->SetViewTargetWithBlend(Pawn, BlendTime);)
}
```


# Look At

Making a Camera look at a target can be done with the help of Find Look At Rotation.
The following example assumes that Actor With Camera has a Camera Component at its local origin,
i.e. with an identity, i.e. all-zero, transformation.
```cpp
#include "Kismet/KismetMathLibrary.h"

void UMyObject::LookAt(AActor* ActorWithCamra, AActor* Target)
{
	const FVector CameraLocation = ActorWithCamera->GetActorLocation();
	const FVector TargetLocation = Target->GetActorLocation();
	FRotator Rotator = UKismetMathLibrary::FindLookAtRotation(CameraLocation, TargetLocation);
	ActorWithCamera->SetActorRotation(Rotator);
}
```
# Camera Shake

See [[Camera Shake]].


# References

- [_Begin Play | Gameplay_ by Epic Games, Samuel Bass @ dev.epicgames.com 2022](https://dev.epicgames.com/community/learning/tutorials/l21z/unreal-engine-begin-play-gameplay)
- [_Camera Framework Essentials for Games_ by Epic Games, Rob Brooks @ dev.epicgames.com 2022 UE 5.0](https://dev.epicgames.com/community/learning/courses/RRr/unreal-engine-camera-framework-essentials-for-games/wv7n/unreal-engine-camera-framework-essentials-for-games-overview)
