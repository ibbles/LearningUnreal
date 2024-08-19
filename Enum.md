An enum is a compile-time list of named values.
Each value is an integer, by default starting from 0 and counting up.
Regular C++ enums are a bit more flexible, but Unreal Header Tool imposes some restrictions.
The number of elements is returned by the `Get number of entries in <ENUM>` node.
Select a random enum element with `Random Integer in Range` and `Get number of entries in <ENUM>` - 1, convert the random Integer to Byte, then finally `Literal enum <ENUM>`.
Enum elements can be converted to String.

Enums can be printed in C++ with:

```c++
StaticEnum<EMyEnum>()->GetNameByValue(EMyEnum::Value)
```
Or
```c++
StaticEnum<EMyEnum>()->GetDisplayNameTextByValue(EMyenum::Value)
```
`StaticEnum<EMyEnum>()` returns an `UEnum*`.

May need `GetNameByValue(int64(SomeEnumValue))`.

A helper:
```c++
template<typename T>
static FString EnumToString(const T Value)
{
    return StaticEnum<T>()->GetDisplayNameStringByValue((int64)Value);
}
```

This can be used to read all the names of an enum's possible values, for example to populate a combo box.
I think this only works for consecutive 0 to N-1 enums, i.e., no fancy `=` in the enum definition.
```cpp
TArray<TSharedPtr<FString>> ComboBoxEntries;
UEnum* MyEnum = StaticEnum<EMyEnum>();
check(MyEnum);
ComboBoxEntries.Reserve(MyEnum->NumEnums);
for (int32 EnumIndex = 0; EnumIndex < MyEnum->NumEnums() - 1; ++EnumIndex)
{
    ComboBoxEntries.Add(
        MakeShareable(new FString(MyEnum->GetNameStringByIndex(EnumIndex))));
}
```

Not sure why it does `-1` in the `for`-loop.

`FindObject<UEnum>;` is the old way to do `StaticEnum`.
Don't know why it was replaced, or when.

In a Blueprint an enum can be converted to either a Name or a String.
When converted to a name the name of the enum will be included.
T.ex. `EMyEnum::MyValue`.
When converted to a string only the name of the value is included.
T.ex. `MyValue`.


# Creating New Enum Types

An enum can be either scoped or unscoped.
A scoped enum is also called a class enum.
A scoped enum places its enum literals in a separate scope.
If the enum is to be known by Unreal Engine, for example to be used in Blueprint or with the `StaticEnum` function, then it must be prefixed with the `UENUM` decorator.
This made available by including `#include "UObject/ObjectMacros.h"`

```cpp
#include "UObject/ObjectMacros.h"

UENUM()
enum EMyUnscopedEnum : uint8
{
    FirstMember,
    SecondMember
};

UENUM()
enum class EMyScopedEnum : uint8
{
    FirstMember,
    SecondMember
};

void Demo()
{
    std::cout << "Accessing an unscoped enum: " << FirstMember;
    std::cout << "Accessing a scoped enum: " << EMyScopedEnum::FirstMember;
}
```

We can add meta data to our enum:
```cpp
UENUM(BlueprintType, Meta=(Bitflags))
```

- BlueprintType: The enum can be used as a variable in a Blueprint.
- Bitflags: Not sure what the effect is, but used to indicate that individual bits have meaning and not the entire value.

We can add meta-data to our enum literals as well:
```cpp
UENUM(BlueprintType)
enum class EMyScopedEnum : uint8
{
    FirstMember UMETA(DisplayName="First member"),
    SecondMember UMETA(ToolTip="My tool tip text.")
    ThirdMember UMETA(Hidden)
};
```


## Enum UPROPERTIES

An enum can be a [[UPROPERTY]] on a U-class.
Unscoped enum must be wrapped in a `TEnumAsByte<E>`.
Therefore, prefer scoped enums, i.e., `UENUM(BlueprintType) enum class EMyScopedEnum : uint8 {}`.

```cpp
UCLASS()
class UMyClass : public UObject
{
    GENERATED_BODY()

    UPROPERTY()
    TEnumAsByte<EMyUnscopedEnum> MyUnscopedEnumBad;

    UPROPERTY()
    EMyScopedEnum MyScopedEnumGood;
}
```
