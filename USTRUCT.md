A macro put before a C++ [[Struct]] to expose it to the Unreal Engine type system.
Such a struct may contain [[Property|Properties]] but not [[Blueprint Callable]] member functions.
Such functions should instead be put in a [[Blueprint Function Library]].
```cpp
USTRUCT(BlueprintType)
struct MYMODULE_API FMyStruct
{
	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "My Struct")
	int32 MyInt;
};
```

For more details on using `struct`s in Unreal Engine, see [[Struct]].
