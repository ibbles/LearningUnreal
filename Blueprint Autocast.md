The [[Blueprint Editor]] can insert implicit casts when connecting two pins of different type.

It is possible to create your own implicit casts for your own [[Blueprint Type]]s by adding functions with the `BlueprintAutocast` [[Function Specifier]] to a [[Blueprint Function Library]] [(1)](https://unreal-garden.com/docs/ufunction/#blueprintautocast).

```cpp
// Unreal Engine includes.
#include "CoreMinimal.h"
#include "Kismet/BlueprintFunctionLibrary.h"

// Project includes.
#include "MyType.h"
#include "MyOtherType.h"

#include "MyBlueprintFunctionLibrary.generated.h"

UCLASS()
class MYMODULE_API UMyBlueprintFunctionLibrary : public UBlueprintFunctionLibrary
	UFUNCTION(
		BlueprintPure, Meta = (BlueprintAutocast, DisplayName = "To MyType"),
		Category = "My Category")
	static FMyType Conv_MyOtherTypeToMyType(FMyOtherType In)
	{
		return FMyType(In);
	}

	UFUNCTION(
		BlueprintPure, Meta = (BlueprintAutocast, DisplayName = "To MyOtherType"), Category = "My Category")
	static FMyOtherType Conv_MyTypeToMyOtherType(FMyType In)
	{
		return FMyOtherType(In);
	}
};
```


# References

- 1: [_UFUNCTION_ > _Blueprint Autocast_ by unreal garden @ unreal-garden.com](https://unreal-garden.com/docs/ufunction/#blueprintautocast)
- 2: [_UFunctions_ @ Epic Games @ dev.epicgames.com/documentation](https://dev.epicgames.com/documentation/en-us/unreal-engine/ufunctions-in-unreal-engine)
