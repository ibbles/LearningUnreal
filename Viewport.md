A Viewport is a window in which something is rendered.
The big area in the middle of the Unreal Editor is the main [[Level Viewport]].
Many sub-editors, such as [[Blueprint Actor Editor]],  [[Static Mesh Editor]] and [[Material Editor]] also contain viewports displaying the opened [[Asset]].

Many viewports support the same set of [[Viewport Camera Controls]].

Some viewports support object selection:
- LMB: Select single object.
- CTRL + LMB: Toggle selection of object.
- CTRL + ALT + LMB-drag: Select in rectangle.
	- Only works in some [[Viewport]]s.

At runtime we can get a hold of the game viewport with
```cpp
UGameViewportClient* GetGameViewport()
{
	UWorld* World = GetWorld();
	if (World == nullptr)
	{
		UE_LOG(
			LogTemp, Warning,
			TEXT("Cannot get game viewport because World is nullptr."));
		return nullptr;
	}
	UGameViewportClient* ViewportClient = World->GetGameViewport();

	return ViewportClient;
}
```

or

```cpp
// Unreal Engine includes.
#include "CoreMinimal.h"
#include "Engine/Engine.h"

UGameViewportClient* GetGameViewport()
{
	if (GEngine == nullptr)
	{
		UE_LOG(
			LogTemp, Warning,
			TEXT("Cannot get game viewport because GEngine is nullptr."));
		return nullptr;
	}
	if (GEngine->GameViewport == nullptr)
	{
		UE_LOG(
			LogTemp, Warning,
			TEXT("Cannot get game viewport because GEngine->GameViewport is nullptr."));
		return nullptr;
	}

	return GEngine->GameViewport;
}
```

Are we guaranteed that these will always return the same Game Viewport client object?