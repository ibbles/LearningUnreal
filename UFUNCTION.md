
The `UFUNCTION` decorator macro exposes a C++ function to the Unreal Engine reflection system.

```c++
UCLASS()
class AMyActor : public AActor
{
    GENERATED_BODY()
public:
    UFUNCTION()
    void MyFunction();
}
```
There are some limitations on what functions can have the `UFUNCTION` decorator.
- Does not support **overloading**, i.e., function member names must be unique.
- Does not support **template**, but there might be a work-around based on custom thunks and/or wildcards. More research needed.
- Does not support **reference return** types. Use a output parameter instead, i.e. `void MyFunction(int& OutParam)`.
- Does not support **FStruct pointer return** types. But I think a struct pointer output parameters, i.e. `FMyStruct*&` works.
- Does not support **UObject reference** parameters. But pointers, and references to pointers, work.

Some of the limitations may be for functions exposed to [[Blueprint Visual Script]] only.

An empty `UFUNCTION` decorator doesn't do much by itself.
We can add [[Function Specifier|Function Specifiers]] to the `UFUNCTION` decorator to enable additional features.

- `BlueprintCallable`: The function can be called from a Blueprint Visual Script.
- `BlueprintPure`: The function can be called from a Visual Script, will note have an execution pin.
	- `const` functions are pure by default. Add `BlueprintPure = False` to turn it into a non-pure node.
- `BlueprintImplementableEvent`: The function is only declared and not implemented in C++, the function definition is provided by a Blueprint class inheriting from the C++ class. I assume `Blueprintable` must be provided on the C++ class for this to make sense.
- `BlueprintNativeEvent`: Like `BlueprintImplementableEvent`, but with a fallback C++ definition in case no Blueprint implementation is made. A separate C++ function with the same name but with `_Implementation` at the end is declared automatically and this is where the C++ implementation should be written. I assume `Blueprintable` must be provided on the class for this to make sense.

A non-C++-`const` `BlueprintCallable` function will have an execution pin.
A C++-`const` `BlueprintCallable` function will not have an execution pin.

The only difference between `BlueprintPure` and `BlueprintCallable` is that `BlueprintCallable` only updates its outputs once when the execution runs through it. `BlueprintPure` will rerun the function and update the output for every single thing that tries to get a value from it.

Most types that can be used in Blueprints can be used for **parameters** and return types.
A parameter taken by reference becomes an **output pin** on the Visual Script node.
To create an output pin pass a by-reference parameter to the function.
If you want it to be an input pin instead then add `UPARAM(Ref)` before the parameter type.
This is useful for modifying the state of a [[USTRUCT]] variable.
(
Though be aware that the Blueprint VM will still copy the instance in and out of the VM's memory, so it only looks like a reference in that we can update the source struct.
This matters if the struct has members that aren't copied by the assignment operator / copy constructor / and/or whatever else the Blueprint VM may decide to do the copy with.
)
```cpp
UFUNCTION(BlueprintCallable, Category = "MyCategory")
void MyFunction(int32& OutputParameter);

UFUNCTION(BlueprintCallable, Category = "MyCategory")
void MyFunction(UPARAM(Ref) FMyType& InOutMyParameter);
```

`UPARAM(Ref)` Can also be put on the return value:
```cpp
UFUNCTION(BlueprintCallable, Category = "MyCategory")
UPARAM(Ref) FMyType& MyGetterFunction();
```

Using `UPARAM` we can also set the name of the output pin:
```cpp
UFUNCTION(BlueprintCallable, Category = "MyCategory")
UPARAM(DisplayName = "Num Widgets") int32 GetNumWidgets();
```
If the function is `static` then the `UPARAM` macro should be placed between the `static` keyword and the return type.

To return a pointer to a UObject, make a parameter that is a reference to a pointer.
```cpp
UFUNCTION(BlueprintCallable, Category = "MyCategory")
void MyFunction(UMyType*& OutReturn);
```


If you have a Function with an Array parameter that you often want to pass an empty array to then you can add the `AutoCreateRefTerm="MyParameter"` Meta Specifier to the function. Then that pin can be left unconnected in a Blueprint graph.
For example:
```cpp
UFUNCTION(BlueprintCallable, Category = "MyCategory", Meta=(AutoCreateRefTerm="MyArray")
float Sum(const TArray<float>& MyArray)
```

# Implement Function In Blueprint Script

`TODO` Move to [[Exposing C++ Functions To Blueprint]].

There are two [[Function Specifier]]s that allow a [[Blueprint Class]] author to implement logic in [[Blueprint Script]]:
- Blueprint Implementable Event
- Blueprint Native Event

The difference between them is that the Native Event variant can have a C++ implementation while the Implementable Event cannot.
From the C++ both appear as a regular function call.

## Blueprint Implementable Event

Example with a Blueprint Implementable Event.

`MyComponent.h`:
```cpp
#pragma once

#include "CoreMinimal.h"
#include "Components/ActorComponent.h"

UCLASS(Blueprintable, Meta=(BlueprintSpawnableComponent))
class MYMODULE_API UMyComponent : public UActorComponent
{
	GENERATED_BODY()

public:
	UFUNCTION(BlueprintImplementableEvent)
	void MyFunction();

	void CallMyFunction();
};
```

`MyComponent.cpp`:
```cpp
#include "MyComponent.h"

void UMyComponent::CallMyFunction()
{
	// Call into Blueprint Script.
	MyFunction();
}
```

To implement the Blueprint Implementable Event in a [[Blueprint Class]] click [[Blueprint Editor]] > [[My Blueprint Panel]] > Functions > Override and the function should be included in the list.
If the function has a return value then it will be added as a [[Blueprint Function]].
If the function does not have a return value, i.e. the return type is `void`, then it will be added as a [[Blueprint Event]] in the [[Event Graph]].

I do not know what happens if the [[Blueprint Class]] doesn't override a Blueprint Implementable Event with a return value.
Is a default constructed value returned?
What if there is no default constructor for the return type?

## Blueprint Native Event

Example with a Blueprint Native Event.

`MyComponent.h`:
```cpp
#pragma once

#include "CoreMinimal.h"
#include "Components/ActorComponent.h"

UCLASS(Blueprintable, Meta=(BlueprintSpawnableComponent))
class MYMODULE_API UMyComponent : public UActorComponent
{
	GENERATED_BODY()

public:
	UFUNCTION(BlueprintNativeEvent)
	void MyFunction();

	void CallMyFunction();
}
```

`MyComponent.cpp`:
```cpp
#include "MyComponent.h"

void UMyFunction::CallMyFunction()
{
	MyFunction();
}

void UMyFunction::MyFunction_Implementation()
{
	// Default logic goes here.
}
```

Notice that the `_Implementation` member function isn't declare in the class definition.
That's weird but OK.

The [[Blueprint Visual Script]] can call the C++ implementation, but that is not by default.
To add a call right-click the event or function node and select Add Call To Parent Function.

See also:
- [[UCLASS]]
- [[Blueprint Function]]
- [[Blueprint Function Library]]


# References

- [_Why I prefer BlueprintImplementableEvent to BlueprintNativeEvent_ by Ben @ benui.ca](https://benui.ca/unreal/implementable-event-or-native-event/)

