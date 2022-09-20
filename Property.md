A Property is a data member that is exposed to the Unreal Engine [[Reflection]] system.
In C++ the data member is marked with the [[UPROPERTY]] decorator.
I think any [[Blueprint Variable|Variable]] added to a Blueprint class also counts as a Property.


# Manipulating Properties in C++

The Property system is exposed to C++ code through the `FProperty` type.
There is one `FProperty` instance for every Property in every `UObject` type, but not for every instance of the `UObject` or `UObject` subclass.
An `FProperty` doesn't hold any data itself, it is a description of how to find the data in an instance of a class.
It holds type and offset information.
Given a pointer to an instance of a `UObject` or a `UObject` subclass and an `FProperty` we can get at the actual data inside the `UObject` instance.
This is done with `FProperty::ContainerPtrToValuePtr`, which returns a typed pointer into the storage of the `UObject` instance.
It does basically `byte* FProperty::ContainerPtrToValuePtr(UObject* Object) { return static_cast<byte*>(Object) + this->Offset; }`.

`FProperty` also contains functions to manipulate value of the type it contains, such as copying them with `FProperty::CopyCompleteValue`.


## Copy From One Property To Another

Example demonstrating how to copy one a value from one Property in one `UObject` instance to another Property in another instance.
We use `uint8` as a store of raw bytes, the data can be any type or size.
It is also possible to specify the type you want directly in the template parameter.
Then you get a typed pointer to the data.

```cpp
UObject* TheObjectThatOwnsTheFirstProperty = ...;
UObject* TheObjectThatOwnsTheSecondProperty = ...;
FProperty* FirstProperty = ...;
FProperty* SecondProperty = ...;
check(FirstProperty->SameType(SecondProperty));
const uint8* SrcPtr = FirstProperty->ContainerPtrToValuePtr<uint8>(TheObjectThatOwnsTheFirstProperty);
uint8* DestPtr = SecondProperty->ContainerPtrToValuePtr<uint8>(TheObjectThatOwnsTheSecondProperty);
FirstProperty->CopyCompleteValue(DestPtr, SrcPtr);
```

