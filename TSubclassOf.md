`TSubclassOf` is used when we want to store a reference to a [[UObject]] class.
Not to an instance of a class, but the class itself.
The data that Unreal's reflection system created when [[Unreal Header Tool]] parsed the C++ code.
Using `TSubclassOf` we can for example create new instances of that class during gameplay.
For example, a level designer can chose what type of ship should spawn at a space port.

```cpp
#pragma once
#include "MyProject/Ship.h"
#include "GameplayFramework/Actor.h"

UCLASS()
class MYPROJECT_API ASpacePort : public AActor
{
	GENERATED_BODY();
public:
	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "MyProject"
	TSubclassOf<AShip> ShipTypeToSpawn;
};
```

The actual [[Spawn]] can be done with `UWorld::SpawnActor`.