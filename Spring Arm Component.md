A [[Scene Component]] designed to attach a [[Camera]] to a [[Pawn]].
Provides smoothness to motions, as opposed to the instant snapping that a fixed third-person camera would produce.

Add a Spring Arm to an [[Actor]] [[Blueprint Class]] from the Components panel > Add menu.

The Spring Arm defines two points: the **base point** and the **head point**.
The two points are visualized with a red line between them when the [[Camera Component]] is selected.
The **base point** should be attached to the thing that is being viewed, typically a [[Mesh]] or something that moves with the [[Mesh]].
Attach the [[Spring Arm Component]] to another [[Scene Component]] in the Component hierarchy.
The base point is defined  by the location of the Spring Arm Component, along with Socket Offset and Target Offset.
It is the point that the [[Camera]] will be looking at.
You can tweak the Spring Arm's local transform to fine-tune the look-at  point, or edit the Socket Offset or Target Offset.
The **head point** is where the [[Camera]] will be attached.
Place a [[Camera Component]] at the head point by making it an attachment child of the Spring Arm.
It is recommended to keep the local transform of the [[Camera Component]] identity.
The responsibility of the Spring Arm is to move the head point so that it follow the base point's movements in a way that works well for the player.
The head point is defined as a combination of the base point, the motion of the base point, and the Spring Arm settings.
The Camera Lag settings in the [[Details Panel]] control how the motion of the base point translate into motion of the head point.

# Camera Orientation

The Spring Arm can inherit the roll, pitch and/or yaw of the owning [[Actor]].
This means that if the [[Controller]] makes the [[Actor]] turn then if yaw  inheriting is enabled then the camera will rotate with it.
If the camera was behind the [[Actor]] before the [[Actor]] turns then it will remain behind the [[Actor]] and another part of the environment will be brought into view.
If yaw inheritance is disabled then the camera will keep its current world orientation as the [[Actor]] turns and we will see a different side of the [[Actor]] while the same part of the environment remains in view.

If Use Pawn Control Rotation is enabled then the Spring Arm takes over rotation from the [[Actor]].
When the player sends a turn input the [[Actor]] will stay still and the camera will orbit instead.
Imagine a character turning its head instead of the whole body.


# Camera Lag

Enabling camera lag makes the [[Camera]] not immediately follow the motion of the [[Actor]], but is allowed to trail behind slightly.
This gives a sense of inertia that feels more natural than a rigid coupling.
The camera follows the [[Actor]] rather then being attached to it.

Camera Lag Speed controls how fast the Spring Arm moves the Camera.
Set this very low and the [[Actor]] will be able to run away from the Camera.
Set it very high and you might as well disable camera lag entirely.
Try values around 5-10 and tweak appropriately.

To prevent the camera from getting too far away from the [[Actor]] you can set Camera Lag Max Distance.
Zero means no limit.

There is the option to add lag for rotation as well,
but this often makes the camera control wobbly and imprecise and can lead to motion sickness so it is not often used.


# Collision Avoidance

The Spring Arm can do collision avoidance for the camera.
These settings are in the Camera Collision category of the [[Details Panel]].
Tick Do Collision Test to enable the feature.
The goal of the collision avoidance algorithm is to keep a occlusion-free view of the base point.
This is done though a [[Line Trace]] between the head point and the base point and if there is an obstacle then the spring arm is temporarily shortened.
The [[Line Trace]] is done with a sphere sweep and the radius of that sphere is controlled with Probe Size.


# Details Panel Settings

The distance the head point is from the base point is controlled with Target Arm Length, in cm.

# Camera On Spring Arm In C++

Example of how to create a [[Character]] subclass that contains a [[Camera Component]] attached to a Spring Arm Component.
`MyCharacter.h`:
```cpp
#pragma once

#include "CoreMinimal.h"
#include "MyCharacter.generated.h"

class UCameraComponent;
class USpringArmComponent;

UCLASS()
class MYMODULE_API AMyCharacter : public ACharacter
{
	GENERATED_BODY()
public:
	AMyCharacter();

	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "Camera")
	USpringArmComponent* SpringArm;

	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "Camera")
	UCameraComponent* Camera;
};
```

`MyCharacter.cpp`:
```cpp
#include "MyCharacter.h"

#include "Camera/CameraComponent.h"
#include "GameFramework/SpringArmComponent.h"
AMyCharacter::AMyCharacter()
{
	SpringArm = CreateDefaultSubobject<USpringArmComponent>(TEXT("SpringArm"));
	SpringArm->SetupAttachment(RootComponent);

	// Set spring arm parameters here, though beware that some recommend that
	// everything be left at their defaults and any configuration be done in
	// a Blueprint class instead where changes can be made much quicker.
	SpringArm->TargetArmLength = 300.0;

	Camera = CreateDefaultSubobject<UCameraComponent>(TEXT("Camera"));
	Camera->SetupAttachment(SpringArm);
}
```
# References

- [_Camera Framework Essentials for Games_ by Epic Games, Rob Brooks @ dev.epicgames.com 2022 UE 5.0](https://dev.epicgames.com/community/learning/courses/RRr/unreal-engine-camera-framework-essentials-for-games/wv7n/unreal-engine-camera-framework-essentials-for-games-overview)
