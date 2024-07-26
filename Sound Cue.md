A Sound Cue is an [[Asset]] that describe how a [[Sound]] should be played.
See also [[MetaSounds]]

# Creating A Sound Cue

Create with [[Content Browser]] > right-click > Sounds > Sound Cue.

## Sound Cue Editor

Sound Cues are edited with the Sound Cue Editor.
It is a graph-based editor that samples [[Sound Wave]]s and optionally modifies the signal in various ways before it is output to the speakers.
Connect nodes together to feed audio into the Output node.

## Playing A Sound Wave

To sample a [[Sound Wave]] add a Wave Player node.
Select a [[Sound Wave]] to play at [[Details Panel]] > Wave Player > Sound Wave.
To simply play the sound, i.e. not processing it in any way, connect the Wave Player output pin to the Output node input pin.

To preview the Sound Cue, press the Play Cue button in the tool bar.


# Playing A Sound Cue In C++

`MyActor.h`:
```cpp
#pragma once

// Unreal Engine includes.
#include "CoreMinimal.h"
#include "GameFramework/Actor.h"

#include "MyActor.generated.h"

class USoundCue;

UCLASS()
class MYMODULE_API AMyActor : public AActor
{
	GENERATED_BODY()

public:
	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "My Actor")
	USoundCue* Sound;

	void PlaySound():
}
```

`MyActor.cpp`:
```cpp
#include "MyActor.h"

// Unreal Engine includes.
#include "Sound/SoundCue.h"

void AMyActor::PlaySound()
{
	// There is also PlaySoundAtLocation.
	UGameplayStatics::PlaySound2D(this, Sound);
	)
}
```

Create a [[Blueprint Class]] inheriting from My Actor and assign a Sound Cue [[Asset]] to the Sound property either at [[Blueprint Class Editor]] > [[Class Defaults]] > [[Details Panel]] > My Actor > Sound or at the corresponding location in the [[Details Panel]] for an instance of the class.


# References

- [_C++ For Unreal Engine (Part 2) | Learn C++ For Unreal Engine | C++ Tutorial For Unreal Engine_ by Nerd's Lesson, A.T. Chamillard @ youtube.com, 2022, UE4.27](https://youtu.be/IYJwU-rB2jA?t=14178)
