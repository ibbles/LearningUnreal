An Actor Component, often just Component, is something that is added to an [[Actor]] to provide some functionality.
The Unreal Engine comes with a larger number of Component types for many types of uses:
- [[Light Source]]
- [[Audio Component]]
- [[Niagara Component]]
- [[Static Mesh Component]]
- [[Skeletal Mesh Component]]
- [[Cable Component]]
- [[Collision Shape]]
- [[Physics Constraint]]
- [[Camera]]
- And many more.

The intention is that developers using Unreal Engine create their own Component types for their specific needs.
There are three main types of Components we can base our own Components on:
- Actor Component (`UActorComponent`)
	- Abstract or internal behavior  or logic that does not have a visual or physical representation.
- Scene Component (`USceneComponent`)
	- A subclass  of Actor Component that adds a transformation and an attachment hierarchy with parent-child relationships.
- Primitive Component (`UPrimitiveComponent`)
	- A subclass of Scene Component that adds geometry information. Used for e.g. rendering and [[Collision]].


# Scene Component

A Component may have a transformation or not.
Components without a transformation inherit from Actor Component, `UActorComponent` in C++.
Components with a transformation inherit from [[Scene Component]], `USceneComponent` in C++, a subclass of Actor Component.
Scene Components exist in a hierarchy within the owning Actor, rooted at the [[Root Component]].
When a Scene Component is transformed all its children will transform with it.
See also [[Transform]].


# Component Destruction

Components can be destroyed by calling the Destroy Component member function.
Until the end of the frame (I think) the Component will still exist but the Is Component Being Destroyed member function will return true.
(
I'm not sure the above is true.
)
I'm not sure what happens if you access an Actor member Component after it has been destroyed.
Is it still safe to call Is Valid on it?


# Ticking

Actor Component has a Tick Component callback that should be disabled for every Component that don't need it,
for runtime performance reasons.
For a Blueprint Component untick the [[Class Defaults]] > Tick > Start With Tick Enabled checkbox.
For a C++ Blueprint Component that don't ever need the tick callback do `PrimaryComponentTick.bCanEverTick = false` in the constructor.
For a C++ Blueprint that only needs ticking sometimes do `PrimaryComponentTick.bStartWithTickEnabled = false` in the constructor.
Some time later, use the `PrimaryComponentTick.SetTickFunctionEnable` function to enable or disable the tick callback.

# Creating A New Actor Component Type

## Blueprint

[[Content Browser]] > right-click > New Blueprint Class > Actor Component, Scene Component, or any of the Actor Component subclasses listed under All Classes.
Give the new Component type a name.
A common naming convention is to prefix the name of Blueprint Components with `BP_`.
Double-click the new [[Asset]] to open the [[Blueprint Component Editor]] where the contents and behavior of the Component can be configured.


## C++

Create a new class with the following content, where `MyComponent` is the name of the new Component type.

See [[Module]], [[UPROPERTY]], [[UFUNCTION]] ... for details on some of the bits in the code below.

`MyComponent.h`:
```cpp
#pragma once

#include <Components/ActorComponent.h>

#include "MyComponent.generated.h"

UCLASS(Meta = (BlueprintSpawnableComponent))
class MYPROJECT_API UMyComponent : public UActorComponent /* Or, for example, USceneComponent. */
{
	GENERATED_BODY()
public:
	UPROPERTY(VisibleAnywhere, BlueprintReadWrite, Category = "My Component")
	double MyData {1.0};

	UFUNCTION(BlueprintCallable, Category = "My Component")
	void MyFunction();
};
```

This will produce a Component that can be added to a [[Blueprint Class]] [[Actor]], be edited from the [[Details Panel]], and be manipulated from [[Blueprint Visual Script]].

If the Component cannot be added to a [[Blueprint Class]], i.e. it doesn't show up in the Add Component list, then check the following;
- Has `BlueprintSpawnableComponent` been added to the `Meta` [[Class Specifier]].
	- Make sure it is spelled correctly, there is no error checking on these.
	- Make sure `BlueprintSpawnableComponent` is inside `Meta` and not directly under `UCLASS`.
- Has the `MYPROJECT_API` [[Module]] export macro been added before the class name?


# References

- [_Adding Components to an Actor_ by Epic Games @ docs.unrealengine.com UE 5.3](https://docs.unrealengine.com/5.3/en-US/adding-components-to-an-actor-in-unreal-engine/)

