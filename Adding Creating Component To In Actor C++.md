In a [[Blueprint Class]] inheriting from [[Actor]] a [[Component]] can be added to the class using the Components panel in the [[Blueprint Actor Editor]].
The equivalent operation in C++ is to call the `CreateDefaultSubobject` function from a `AActor` subclass' constructor.
The name of the new [[Component]] is passed to `CreateDefaultSubobject`.
`CreateDefaultSubobject` returns a pointer to the created [[Component]].
The pointer should be assigned to a [[Property]] member variable.

`CreateDefaultSubobject` can only be called from the Actor's constructor.
The `Default` part of `CreateDefaultSubobject` means that all instances of this [[Actor]] will contain an instance of the created [[Component]] type.

`MyActor.h`:
```cpp
#include "CoreMinimal.h"
#incldue "GameFramework/Actor.h"
#include "MyActor.generated.h"

class UMyComponent;

UCLASS()
class MYPROJECT_APT AMyActor : public AActor
{
	GENERATED_BODY();

public:
	UPROPERTY(EditDefaultsOnly, Category = "My Actor")
	UMyComponent* MyComponent;
};
```

`MyActor.cpp`:
```cpp
#include "MyActor.h"
#include "MyComponent.h"

AMyActor::AMyActor()
{
	MyComponent = CreateDefaultSubobject<UMyComponent>(TEXT("My Component"));
}
```


If the [[Component]] is a [[Scene Component]] instead of a plain [[Actor Component]] then creating it isn't enough.
A [[Scene Component]] must be placed somewhere within the attachment hierarchy and it must have a transformation.
We must also make sure we have a [[Root Component]] in our [[Actor]].
There is one by default, but we can replace it with another [[Scene Component]] instance.

Example where the new [[Scene Component]] becomes the new root:
`MyActor.cpp`:
```cpp
AMyActor::AMyActor()
{
	MySceneComponent = CreateDefaultSubobject<UMySceneComponent>(TEXT("My Scene Component"));
	RootComponent = MySceneComponent; // TODO Is there a SetRootComponent function we should call instead?
}
```
A [[Scene Component]] that is set to be the [[Root Component]] should usually not be given a transformation.

Example where the new [[Scene Component]] is attached to the current root and given a transformation:
`MyActor.cpp`:
```cpp
AMyActor::AMyActor()
{
	MySceneComponent = CreateDefaultSubobject<UMySceneComponent>(TEXT("My Scene Component"));
	MySceneComponent->SetupAttachment(RootComponent);
	MySceneComponent->SetRelativeLocation(FVector(0.0, 0.0, 0.0));
}
```

A [[Component]] created like this will be marked `(Inherited)` in [[Blueprint Class]] subclasses.
An inherited [[Component]] cannot be removed from the [[Blueprint Actor Editor]].


# References

- [_Breaking Down the Components of Gameplay_ > _Component Design Choices_ by Epic Games, Rob @ dev.epicgames.com 2021](https://dev.epicgames.com/community/learning/courses/mo/unreal-engine-breaking-down-the-components-of-gameplay/aDO/unreal-engine-component-design-choices)
