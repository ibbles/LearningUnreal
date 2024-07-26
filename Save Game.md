A Save Game is an object that is written to persistent storage and that can be restored later.
We save and load game progress by writing the game state we need to a Save Game object and then writing that to persistent storage.
To later load the game state we read the Save Game object.

Save Games are stored in slots.
Each slot holds one Save Game. (I assume, but not sure what the User Index parameter is.)

Each save has a User Index.
I don't know what that is yet.

There is an asynchronous variant of loading.


# Save Game Class In C++

The Save Game class contains the data that is stored to persistent storage.

`MySaveGame.h`:
```cpp
#pragma once

// Unreal Engine includes.
#include "CoreMinimal.h"
#include "GameFramework/SaveGame.h"

#include "MySaveGame.generated.h"

UCLASS()
class MYMODULE_API UMySaveGame : public USaveGame
{
	GENERATED_BODY();

public:
	UPROPERTY(VisibleAnywhere, Category = "My Save Game")
	int32 Score;
};
```

`MySaveGame.cpp`:
```cpp
#include "MySaveGame.h"
```

The implementation file don't need any code since the Save Game class is just a data container.


## Saving A Save Game

Example where a [[Game State]] saves a score to a save game.

`MyGameState.cpp`:
```cpp
#include "MyGameState.h"

// Project includes.
#include "MySaveGame.h"

// Unreal Engine includes.
#include "Kismet/GameplayStatics.h"


bool UMyGameState::SaveState()
{
	UMySaveGame* Save = Cast<UMySaveGame>(UGameplayStatics::CreateSaveGameObject(UMySaveGame::StaticClass()));
	Save->Score = Score;
	UGamePlayStatics::SaveGameToSlot(Save, TEXT("MySlot"), 0);
}
```

## Loading A Save Game

Example where a [[Game State]] loads a score from a save game.

`MyGameState.cpp`:
```cpp
#include "MyGameState.h"

// Project includes.
#include "MySaveGame.h"

// Unreal Engine includes.
#include "Kismet/GameplayStatics.h"


bool UMyGameState::LoadState()
{
	UMySaveGame* Save = Cast<UMySaveGame>(UGameplayStatics::LoadGameFromSlot(TEXT("MySlot"), 0));
	if (Save == nullptr)
	{
		return false;
	}

	Score = Save->Score;
}
```

# References

- [_C++ For Unreal Engine (Part 2) | Learn C++ For Unreal Engine | C++ Tutorial For Unreal Engine_ by Nerd's Lesson, A.T. Chamillard @ youtube.com, 2022, UE4.27](https://youtu.be/IYJwU-rB2jA?t=18634)
