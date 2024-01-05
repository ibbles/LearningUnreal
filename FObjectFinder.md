`FObjectFinder` is used to find and load assets in C++.
It is part of [[`ConstructorHelpers`]], which means that it should only be used from [[UObject]] constructors.

A recommendation is to not hard-code string paths in C++ code.
It makes it difficult to customize from Blueprint classes and  it is easy for the paths to become out-of-date when assets are moved.

Example assigning a [[Static Mesh Asset]] to a [[Static Mesh Component]]:
```cpp
// MyObject.h:
UCLASS()
class MYMODULE_API UMyObject : public UObject
{
	GENERATED_BODY()

	UPROPERTY(VisibleAnywhere, BlueprintReadOnly, Category = "My Object")
	UStaticMeshComponent* MyMesh;

public:
	UMyObject();
};

// MyObject.cpp:
UMyObject::UMyObject()
{
	static ConstructionHelpers::FObjectFinder<UStaticMesh> MeshAsset(TEXT("/Game/MyGame/Meshes/MyMesh.MyMesh"));
	MyMesh = CreateDefaultSubobject<UStaticMeshComponent>("MyMesh");
	MyMesh->SetStaticMesh(MeshAsset.Object);
	MyMesh->SetupAttachment(RootComponent);
}
```


# References

- [_Balancing Blueprint and C++ in Game Development_ > _Referencing Assets_ by Epic Games, Rob. 2021](https://dev.epicgames.com/community/learning/courses/bY/unreal-engine-balancing-blueprint-and-c-in-game-development/kG8/referencing-assets)
