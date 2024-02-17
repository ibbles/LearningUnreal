In Unreal Engine a c++ Interface is more than just an abstract base class.
It is a member function system that ties into with the Blueprint runtime,
making it possible to implement the functions declared by the C++ Interface in [[Blueprint Visual Script]].

There are two parts to a C++ Interface:
- Interface declaration.
- Interface definition.

I don't know if these are the correct words. I know what I mean.

The interface declaration is there to let the engine know that an interface with a specific name exists.
The interface definition is where we list the functions that the interface contains.
The functions should be decorated with the [[UFUNCTION]] macro with the
- `BlueprintNativeEvent`
- `BlueprintCallable`
[[Function Specifier]]s.
There may be alternatives to `BlueprintNativeEvent`,
maybe `BlueprintEvent` for when the event should be implemented in Blueprint instead of C++.

The functions listed in the interface definition should not be implemented.
An implementation will be provided by [[Unreal Build Tool]].
If a C++ function implementation is needed then it should be named with the `_Implementation` suffix.
The generated interface function implementation will determine if a [[Blueprint Visual Script]] or C++ function should be called and call it.

The interface declaration class is decorated with the `UINTERFACE` macro and the name should start with `U`.
The interface definition class should have the same name as the declaration but with `I` instead of `U` at the start.

`MyInterface.h`:
```cpp
#pragme once
#include "" // TODO What should go here?
#include "MyInterface.generated.h" // TODO I assume this is needed. Check to make sure.

// Interface declaration.
UINTERFACE(MinimalAPI)
class UMyInterface : public UInterface
{
	GENERATED_BODY()
}

// Interface definition.
class MYMODULE_API IMyInterface
{
	GENERATED_BODY()

public:
	UFUNCTION(BlueprintNativeEvent, BlueprintCallable, Category = "My Interface")
	void MyFunction();
};
```


A class implementing the interface should inherit from the interface definition.
Any C++ definition of a interface function should have `_Implementation` suffixed to the function name.

`MyActor.h`:
```cpp
#pragma once
#include "Gameframework/Actor.h"
#include "MyInterface.h"

UCLASS(BlueprintType)
class MYMODULE_API AMyActor : public AActor, public IMyInterface
{
	GENERATED_BODY()
public:
	UFUNCTION(BlueprintNativeEvent, BlueprintCallable, Category = "My Interface")
	void MyFunction();
};
```

`MyActor.cpp`:
```cpp
#include "MyActor.h"

void AMyActor::MyFunction_Implementation()
{
	// Your logic here.
}
```