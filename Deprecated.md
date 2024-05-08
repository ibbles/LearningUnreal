# Property
When a [[Property]] has been deprecated any use of it in a Blueprint will result in a warning.

(Are deprecated properties still serialized?)

To mark a [[Property]] deprecated you rename it to include the `_DEPRECATED` suffix and add the `DeprecatedProperty` and `DeprecationMessage` [[Meta Property Specifier|Property Specifiers]].

```c++
UCLASS()
class MYMODULE_APT UMyClass : public UObject
{
	GENERATED_BODY()
public:
	UPROPERTY(meta=(DeprecatedProperty, DeprecationMessage="Don't use this anymore, use something else instead."))
	int32 MyProperty_DEPRECATED;
};
```

# Class

To mark a [[UClass]] deprecated add the `Deprecated` [[Class Specifier]] and rename it to include the `DEPRECATED_` prefix, still keeping the single-letter  type-prefix first.

A deprecated class will not be serialized, so make sure you re-implement any functionality you want to keep in some other way before you save and close the Unreal Editor.

```c++
UCLASS(Deprecated)
class MYMODULE_API UDEPRECATED_MyClass : public UObject
{
	GENERATED_BODY()
public:
	UDEPRECATED_MyClass();
};
```

# References

 - [_Metadata usable in UPROPERTY_ by Epic Games @ docs.unrealengine.com](https://docs.unrealengine.com/5.3/en-US/API/Runtime/CoreUObject/UObject/UM_3/)
 - [_DeprecatedProperty_ by Ben @ benui.ca](https://benui.ca/unreal/uproperty/#deprecatedproperty)
 - [_Deprecating UProperties and UFunctions_ by  @ ikrima.dev ikrima 2023](https://ikrima.dev/ue4guide/engine-programming/uobjects/deprecating-uproperties-ufunctions/)
