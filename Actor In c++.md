An [[Actor]] type can be defined in C++ here we will build an [[Actor]] step-by-step.


# Smallest Possible

Here is an example of an Actor defined in C++.
`MyActor.h`:
```cpp
#pragma once

// Unreal Engine includes.
#include "CoreMinimal.h"
#include "GameFramework/Actor.h"

#include "MyActor.generated.h"

UCLASS()
class MYMODULE_API AMyActor : public AActor
{
	GENERATED_BODY()

publc:
	AMyActor();
};
```

`MyActor.cpp`:
```c++
#include "MyActor.h"

AMyActor::AMyActor()
{
	PrimaryActorTick.bCanEverTick = false;
}
```


# Property

A [[Property]] is added to the Actor by marking a data member with the [[UPROPERTY]] decorator.
```c++
UPROPERTY()
int32 MyInt {0};
```

A [[Property]] an be visible / editable independently from the [[Details Panel]] and [[Blueprint Script]].
This is controlled using a [[Property Specifier]], which goes in the parameter list of the [[UPROPERTY]] decorator.
To enable both read and write for both the [[Details Panel]] and [[Blueprint Script]]:
```
UPROPERTY(EditAnywhere, BlueprintReadWrite)
int32 MyInt {0};
```


# Actor Components

An [[Actor Component]] is a piece of self-contained functionality we can add to an [[Actor]].
For example, rendering a static, i.e. non-animated, triangle mesh is functionality provided by the [[Static Mesh Component]].
By adding a [[Static Mesh Component]] to our [[Actor]] we give the [[Actor]] the ability to render itself.

In `MyActor.h`:
```c++
UPROPERTY(VisibleAnywhere)
TOBjectPtr<UStaticMeshComponent> VisualMesh;
```

In `MyActor.cpp`, in the constructor.
```c++
VisualMesh = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("Mesh"));
VisualMesh->SetupAttachment(RootComponent);

static ConstructorHelpers::FObjectFinder<UStaticMesh> SphereAsset(TEXT("/Script/Engine.StaticMesh'/Engine/BasicShapes/Sphere.Sphere'"));

if (SphereAsset.Succeeded())
{
 VisualMesh->SetStaticMesh(SphereAsset.Object);
 VisualMesh->SetRelativeLocation(FVector(0.0f, 0.0f, 0.0f));
}
```

# References

- 1: [_Programming Quick Start_ by Epic Games @ dev.epicgames.com/documentation](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-engine-cpp-quick-start)
- 2: [_Unreal Engine C++ Complete Guide_ by Tom Looman @ https://www.tomlooman.com/ 2023](https://www.tomlooman.com/unreal-engine-cpp-guide/#C_Syntax_Symbols)


