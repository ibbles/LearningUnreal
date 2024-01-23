A struct is a container for data in the form of named attributes.
It can be created either in C++ or Blueprint.
Make a C++ struct [[Blueprint]]-accessible by adding the [[USTRUCT]] decorator with the `BlueprintType` specifier.
A Blueprint accessible C++ struct cannot have [[Blueprint Callable]] member functions.
Functions can instead be put in a [[Blueprint Function Library]].

C++:
```cpp
USTRUCT(BlueprintType)
struct MYMODULE_API FMyStruct
{
	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "My Struct")
	int32 MyInt;
};


UCLASS(BlueprintType)
class MYMODULE_API UMyStruct_FL : public UBlueprintFunctionLibrary
{
	UFUNCTION(BlueprintCallable)
	static void Increment(UPARAM(Ref) FMyStruct& Data)
	{
		++Data.MyInt;
	}
};
```