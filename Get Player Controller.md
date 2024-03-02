A static function to get the [[Player Controller]] for a particular player.

Called from a member function of any [[UObject]]:
```cpp
#include "Kismet/GameplayStatics.h"

void UMyObject::MyFunction()
{
	// Or whatever player you want to do something with.
	int32 PlayerIndex {0};

	APlayerController* PlayerController =
		UGameplayStatics::GetPlayerController(this, PlayerIndex);)
}
```
