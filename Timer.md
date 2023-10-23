A Timer is something that waits for a predetermined amount of time and then performs some action.
For a [[Blueprint Timer]] the action is to trigger a [[Blueprint Event]].
For a C++ timer the action is to call a callback function.

Example code for how to setup a timer when inside an [[Actor]].
```cpp
#pragma once
#include "GameplayFramework/Actor.h"

UCLASS()
class MYPROJECT_API AMyActor : public AActor
{
	GENERATED_BODY();
public:
	AMyActor();
private:
	void OnTimer();
};
```

```cpp
#include "MyProject/MyActor."

AMyActor::AMyActor()
{
	FTimerHandle Timer;
	const float Seconds {10.0f};
	GetWorldTimerManager().SetTimer(
		Timer, this, &AMyActor::OnTimer, Seconds);
}

void AMyActor::OnTimer()
{
	UE_LOG(LogTemp, Display, TEXT("Time's up."));
}
```

If you need to manipulate the timer after it has been started then you can save the timer handle for later use.

```cpp
#pragma once
#include "GameplayFramework/Actor.h"

UCLASS()
class MYPROJECT_API AMyActor : public AActor
{
	GENERATED_BODY();
public:
	AMyActor()
	{
		const float Seconds {10.0f};
		GetWorldTimerManager().SetTimer(
			Timer, this, &AMyActor::OnTimer, Seconds);
	}
	
	void CancelTimer()
	{
		GetWorldTimerManager().ClearTimer(Timer);
	}
private:
	void OnTimer();
	
	FTimerHandle Timer;
};