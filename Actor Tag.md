An [[Actor]] Tag is a string label that is put on an [[Actor]].
Set at [[Details Panel]] > Actor > Advanced > Tags, which is an array of strings.

Find all [[Actor]]s with a particular tag with
```cpp
#include "Kismet/GameplayStatics.h"

void AMyActor::MyFunction()
{
	TArray<AActor*> Actors;
	UGameplayStatics::GetAllActorsWithTag(GetWorld(), TEXT("MyTag"), Actors);
}
```


# References

- [_C++ For Unreal Engine (Part 2) | Learn C++ For Unreal Engine | C++ Tutorial For Unreal Engine_ by Nerd's Lesson, A.T. Chamillard @ youtube.com, 2022, UE4.27](https://youtu.be/IYJwU-rB2jA?t=18185)
