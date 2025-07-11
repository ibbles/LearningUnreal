A Blueprint Function Library is a collection of C++ functions that can be called from [[Blueprint Visual Script]].
Can be used to provided member-function-like functionality to [[Struct]] types.

```cpp
#pragma once

// Unreal Engine includes.
#include "CoreMinimal.h"
#include "Kismet/BlueprintFunctionLibrary.h"

#include "MyStruct.generated.h"

USTRUCT(BlueprintType)
struct MYMODULE_API FMyStruct
{
	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "My Struct")
	int32 MyInt;
};


UCLASS(BlueprintType)
class MYMODULE_API UMyStruct_FL : public UBlueprintFunctionLibrary
{
	GENERATED_BODY()

	UFUNCTION(BlueprintCallable, BlueprintPure, Category = "My Category")
	static void Increment(UPARAM(Ref) FMyStruct& Data)
	{
		++Data.MyInt;
	}
};
```

