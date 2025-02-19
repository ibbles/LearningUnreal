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


# Struct In Blueprint

We can read the values of the struct members with the Break STRUCT function.
Select the node and click Details panel > Hide Unconnected Pins to cut down on the clutter.

We can set values of struct members with the Set Members In STRUCT function.

Beware that structs owned by C++ classes are copied in and out of the [[Blueprint]] virtual machine every time they are accessed in a [[Blueprint Script]].


# "Virtual" Functions With `TStructOpsTypeTraits`

Unreal Engine contains a lot of virtual functions that our own subclasses can override to customize the behavior of our application.
While C++ does support virtual functions on structs, Unreal Engine does not.
Instead it uses a type traits based system to signal that a particular struct provides a customization of a particular function.
This is called the struct's feature set.
The type traits base class is `TStructOpsTypeTraitsBase2` and we configure the feature set of our struct type by inheriting from that base class, templating it for our struct type, and declaring enum literals that are either `true` or `false` using a set of predefined literal names.
You can see the list of supported literal names in `TStructOpsTypeTraitsBase2`,
which can be found in `$UE_ROOT$/Engine/Source/Runtime/CoreUObject/Public/UObject/Class.h`.

The following example shows how to tell Unreal Engine that `MyStruct` has the equality operator and that this operator should be used when comparing two instances of `MyStruct` for equality.

`MyStruct.h`:
```cpp
#pragma once

// Unreal Engine includes.
#include "CoreMinimal.h"
#include "UObject/Class.h"

#include "MyStruct.generated.h"

USTRUCT()
struct MYMODULE_API FMyStruct
{
	GENERATED_BODY()

	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "My Struct")
	double Value {0.0};

	bool operator==(const FMyStruct& Other)
	{
		return Value == Other.Value;
	}
};

template<>
struct TStructOpsTypeTraits<FMyStruct> : public TStructOpsTypeTraitsBase2<FMyStruct>
{
	enum
	{
		WithIdenticalViaEquality = true;
	};
};
```

Unreal Engine contains engine code similar to the following to check the feature flag and call the associated function, if available.
The following is a reduced version of the member function `Identical`, part of a class template, where `CPPSTRUCT` is a template type parameter that in our case would be `FMyStruct`:
```cpp
template <typename CPPSTRUCT>
bool Identical(const void* A, const void* B, bool bOutResult)
{
	// Other code.
	
	if constexpr (TStructOpsTypeTraits<CPPSTRUCT>::WithIdenticalViaEquality)
	{
		bOutResult = (*(const CPPSTRUCT*)A == *(const CPPSTRUCT*)B);
		return true;
	}
	
	/// Other code.
}
```
You can find the complete implementation in `$UE_ROOT$/Engine/Source/Runtime/CoreUObject/Public/UObject/Class.h`.

Other useful "virtual" functions include serialization control.
The following example demonstrate how we can initialize our struct from a previously serialized `double` or `float`, and how to handle changes in [[Property]] semantics.
The semantic change is that we now want to store the inverse value of what we used to, so any old value should be inverted in `Serialize`.
The serialization code is not quite complete, see [[Serialization]] for details on how to set up a custom version for you project.

`MyStruct.h`:
```cpp
#pragma once

// Unreal Engine includes.
#include "CoreMinimal.h"
#include "UObject/Class.h"

#include "MyStruct.generated.h"

USTRUCT()
struct MYMODULE_API FMyStruct
{
	GENERATED_BODY()

	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "My Struct")
	double NewValue {0.0};

	bool Serialize(FArchive& Archive);
	bool SerializeFromMismatchedTag(struct FPropertyTag const& Tag, FStructuredArchive::FSlot Slot);

private:
	UPROPERTY(Meta = (DeprecatedProperty, DeprecationMessage = "Use NewValue instead."))
	double Value_DEPRECATED {0.0};
};

template <>
struct TStructOpsTypeTraits<FMyStruct> : public TStructOpsTypeTraitsBase2<FMyStruct>
{
	enum
	{
		// Tell Unreal Engine that there is a Serialize member function.
		WithSerializer = true,

		// Tell Unreal Engine that there is a SerializeFromMismatchedTag member function.
		WithStructuredSerializeFromMismatchedTag = true
	};
};
```

`MyStruct.cpp`:
```cpp
#include "MyStruct.h"

bool FMyStruct::Serialize(FArchive& Archive)
{
	if (Archive.IsLoading() || Archive.IsSaving())
	{
		UScriptStruct* Struct = FMyStruct::StaticStruct();
		Struct->SerializeTaggedProperties(Archive, reinterpret_cast<uint8*>(this), Struct, nullptr);
	}

	// See Serialization.md for details on custom version handling.
	Archive.UsingCustomVersion(FMyCustomVersion::GUID);
	if (Archive.IsLoading() && Archive.CustomVer(FMyCustomVersion::GUID) < FMyCustomVersion::NewValue)
	{
		// Found an old value, invert it to conform to the new sematics and store in NewValue.
		NewValue = 1.0 / Value_DEPRECATED;
	}

	return true;
}

bool FMyStruct::SerializeFromMismatchedTag(
	const FPropertyTag& Tag,
	FStructuredArchive::FSlot Slot)
{
	// The engine is loading into a FMyStruct but the archive contains something else.
	// If that something else is a double or a float then we can initialize the
	// FMyStruct from it.
	
	if (Tag.Type == NAME_DoubleProperty)
	{
		Slot << NewValue;
		return true;
	}
	else if (Tag.Type == NAME_FloatProperty)
	{
		float Restored;
		Slot << Restored;
		NewValue = static_cast<double>(Restored);
		return true;
	}
	return false;
}
```

I do not know how `Serialize` and `SerializeFromMismatchedTag` interact.
Are we sure that `SerializeFromMismatchedTag` should write to `NewValue` and not `Value_DEPRECATED`?
Can we check the custom version stored in the `FArchive` from the Tag and Slot given to `SerializeFromMismatchedTag`?
Consider a pair of [[UObject]] subclasses that contain a `double` each, one stored in one [[Level]] and the other in another [[Level]].
The two [[Level]]s are serialized, i.e. we open them in Unreal Editor and save.
Then we change the semantics, i.e. we store the inverse value instead, and update the value of one of the `double`s but we don't yet get around to changing the other.
Then we change the type of the [[Property]]s to be `FMyStruct` instead of `double`.
Now when we load either [[Level]] there is no way of knowing if `FMyStruct` instances in this particular [[Level]] has been saved with the new semantics or not.


# Struct Type Descriptions With `TStructOpsTypeTraits`

Not all enum literals in `TStructOpsTypeTraits` are associated with a function, some state facts about the type.
The following example creates a struct type and tells Unreal Engine that instances of this type can be initialized by filling it's memory with zeroes.
There are many similar flags in `TStructOpsTypeTraits`.

`MyStruct.h`:
```cpp
#pragma once

// Unreal Engine includes.
#include "CoreMinimal.h"
#include "UObject/Class.h"

#include "MyStruct.generated.h"

USTRUCT()
struct MYMODULE_API FMyStruct
{
	GENERATED_BODY()

	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "My Struct")
	double Value {0.0};
};

template<>
struct TStructOpsTypeTraits<FMyStruct> : public TStructOpsTypeTraitsBase2<FMyStruct>
{
	enum
	{
		WithZeroConstructor = true;
	}
};
```