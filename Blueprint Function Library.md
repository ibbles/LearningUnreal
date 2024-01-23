A Blueprint Function Library is a collection of C++ functions that can be called from [[Blueprint Visual Script]].
Can be used to provided member-function-like functionality to [[Struct]] types.

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

