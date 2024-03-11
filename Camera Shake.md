
Created by creating a [[Blueprint Class]] inheriting from Camera Shake Base.
The Camera Shake Base [[Asset]] references a Root Shake Pattern.
There are a few that comes with the engine.
Pick one from the drop-down and settings for that particular pattern will appear beneath the Root Shake Pattern drop-down.

Rotation oscillation can cause motion sickness.

Try with location amplitude and frequency  both set to a few tens,
should give a decent start.

A Camera Shake can be either single-shot or continuous.

# Single-Shot Camera Shake

To trigger a camera shake call the Play World Camera Shake function.
In C++ it is available in `UGameplayStatics`.
Set the Shake input to your Camera Shake Base [[Blueprint Class]].
Epicenter is where in the world the shake-causing event happened.
There are three inputs that control camera shake strength based on camera location: Epicenter, Inner Radius, and Outer Radius.
The camera shake will be more pronounced when the camera is closer to the epicenter.
The shake amount is at its maximum when the camera is within the inner radius from the epicenter.
The shake amount decreases for camera located between the inner radius and the outer radius.
Cameras outside the outer radius does not shake at all.

# Continuous Camera Shape

To start a continuous camera shake you need access to a [[Player Camera Manager]].
Can be accessed with Get Player Controller > Player Camera Manager.
[[Player Camera Manager]] has the Start Camera Shake function.
Set the Shake Class input to the Camera Shake Base [[Blueprint Class]] to  play.
Start Camera Shake returns a reference to a Camera Shake Base instance that can be used to control the shake.
To stop the shake pass the Camera Shake Base instance reference to [[Player Camera Manager]]'s Stop Camera Shake function.
The Stop Camera Shake has an Immediate input pin that I'm not sure what it means when set to false.
I guess the camera shake will end after the current cycle finishes,
i.e. after at most Timing > Duration seconds from the Camera Shake Base settings.
If that is set to 0.0, which means infinite, then we must set Immediate to true for the shake to stop.

# Triggering Camera Shake From C++ 

`MyActor.h`:
```cpp
#pragma once

#include "GameFramework/Actor.h"

class UCameraShakeBase;

UCLASS()
class MYMODULE_API AMyActor : AActor
{
	GENERATED_BODY()

public:
	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Camera Shake")
	TSubclassOf<UCameraShakeBase> SingleShotShake;

	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Camera Shake")
	TSubclassOf<UCameraShakeBase> ContinuousShake;

	void TriggerSingleShotShake();
	void StartContinuousShake();
	void StopContinuousShake();

protected:
	UPROPERTY(BlueprintReadWrite, Category = "Camera")
	UCameraShakeBase* CurrentContinuousShake {nullptr};
};
```
`MyActor.cpp`:
```cpp
#include "MyActor.h"

#include "Camera/PlayerCameraManager.h"
#include "Engine/World.h"
#include "GameFramework/PlayerController.h"
#include "Kismet/GameplayStatics.h"

void AMyActor::TriggerSingleShotShake()
{
	if (SingleShotShake == nullptr)
		return;

	const float InnerRadius {300};
	const float OuterRadius {1000};
	UGameplayStatics::PlayWorldCameraShake(
		this, SingleShotShake, GetActorLocation(), InnerRadius, OuterRadius);
}

void AMyACtor::StartContinuousShake()
{
	if (ContinuousShake == nullptr)
		return;

	float Scale {1.0f}
	APlayerCameraManager* CameraManager = GetWorld()->GetFirstPlayerController()->PlayerCameraManager;
	CurrentContinuousShake = CameraManager->StartCameraShake(ContinuousShake, Scale);
}

void AMyActor::StopContinuousShake()
{
	if (CurrentContinuousShake == nullptr)
		return;

	APlayerCameraManager* CameraManager = GetWorld()->GetFirstPlayerController()->PlayerCameraManager;
	CameraManager->StopCameraShake(CurrentContinuousShake, /*Immediate*/ true);
	CurrentContinuousShake = nullptr;
}
```

# Camera Shake Source Actor

The Camera Shake Source Actor can be  used to test camera shake settings.
Create a Camera Shake Source Actor in the level and in the [[Details Panel]] set Camera Shake to your Camera Shake Base [[Blueprint Class]].
Set inner and outer radius under the Attenuation category the same as you set the corresponding inputs on the Play World Camera Shake node.
In from the triple-dash drop-down menu in the top-left of the [[Level Viewport]] enable:
- Realtime.
- Allow Camera Shakes.

Open Top Menu Bar > Window > Cinematics > Camera Shake Previewer.
This opens a window that lists the Camera Shake Source Actors in the leve.
Click the Play/Stop All button to run the camera shake.
Move around the level, at different distances from the Camera Shake Source Actor, to see how the  camera shake is at that distance.
If either of Realtime or Allow Camera Shakes isn't enabled then a message saying so will be displayed at the bottom of the window.


# References

- [_Camera Framework Essentials for Games_ by Epic Games, Rob Brooks @ dev.epicgames.com 2022 UE 5.0](https://dev.epicgames.com/community/learning/courses/RRr/unreal-engine-camera-framework-essentials-for-games/wv7n/unreal-engine-camera-framework-essentials-for-games-overview)
