See also [[Custom Node Graph Editor References]] for a summary of other sources on custom node graph editors.

A node graph editor is an editor used to edit an [[Asset]] that contain graph data.
Examples of built-in graph editors are the [[Blueprint Visual Script Editor]], the [[Material Editor]], and the [[Sound Cue Editor]].
For a description of how the [[Sound Cue Editor]] has been built, see [[Sound Cue Node Graph Editor]].

This note describe how to build a custom node graph editor for a new project-specific asset type.
The example asset is a transportation network.
The network consists of facilities and transportation channels.
A facility can have zero or more inputs and zero or more outputs.
Each facility input and output is of a particular kind.
A facility output can be connected to a facility input via a channel.
A facility output may only be connected to a facility input of the same kind.
Some facilities support multiple input and/or output channels.

Example facilities are
- Factory.
	- No inputs.
	- Outputs items on a conveyor.
- Packager.
	- Inputs items on a conveyor.
	- Outputs packages in a box truck.
- Warehouse.
	- Inputs packages in a box truck.
	- Outputs packages in a box truck.
- Port.
	- Inputs packages in a box truck.
	- Outputs containers on a ship.
	- Inputs containers on a ship.
	- Outputs packages in a box truck.
- Truck stop.
	- Inputs packages in a box truck.
	- Outputs containers on a container truck.
	- Inputs containers on a container truck.
	- Outputs packages in a box truck.
- Store.
	- Inputs packages in a box truck.

The channels are
- Conveyor. Transports items.
- Box truck. Transports packages.
- Ship. Transports containers.
- Container truck. Transports containers.

Important engine classes.
These are the classes that we will either use or subclass to build our custom node graph editor.
- `UObject`
	- Base class for our Transportation Network asset.
- `UEdGraph`
- `UEdGraphNode`
- `UEdGraphPin`
- `UEdGraphEditor`

Important custom classes.
These are the classes what we will write for our custom graph node editor.
- `UTransportationNetwork`
	- Inherits from `UObject`.
	- The asset type.


# `UTransportationNetwork`

This is our main [[Asset]] type.
For more information on custom [[Asset]] types, see [[Custom Asset]].

Users will be able to create instances of this class from the [[Content Browser]].
It will contain a list of facilities and how they are connected.
To start off create a placeholder asset.
`Source/MyProject/Public/TransportationNetwork.h`:
```cpp
// Unreal Engine includes.
#include "UObject/Object.h"

#include "TransportationNetwork.generated.h"

UCLASS(BlueprintType)
class UNREALTESTBED_API UTransportationNetwork : public UObject
{
    GENERATED_BODY()
public:
    UPROPERTY(VisibleAnywhere, BlueprintReadOnly)
    double MyValue;
};
```

This is enough to create a [[Blueprint Variable]] that references a Transportation Network,
but it is not yet possible to create actual Transportation Network instances for the reference to reference.


# `UTransportationNetworkFactoryNew`

This factory is what makes it possible for the user to create new instances of the asset from the [[Content Browser]].
For more information on factories, see [[Custom Asset]].

`Source/MyProject/Public/TransportationNetworkFactoryNew.h`:
```cpp
#pragma once

// Unreal Engine includes.
#include "Factories/Factory.h"

#include "TransportationNetworkFactoryNew.generated.h"

UCLASS()
class UTransportationNetworkFactoryNew : public UFactory
{
    GENERATED_BODY()
public:
    //~ Begin UFactory Interface
    virtual UObject* FactoryCreateNew(
	    UClass* InClass, UObject* InParent, FName InName,
	    EObjectFlags InFlags, UObject* InContext,
	    FFeedbackContext* InWarn) override;

    virtual bool ShouldShowInNewMenu() const override;
    //~ End UFactory Interface
};
```

`Source/MyProject/Private/TransportationNetworkFactoryNew.h`:
```cpp
#include "TransportationNetworkFactoryNew.h"

UTransportationNetworkFactoryNew::UTransportationNetworkFactoryNew(
	const FObjectInitializer& ObjectInitializer)
    : Super(ObjectInitializer)
{
    SupportedClass = UTransportationNetwork::StaticClass();

    bCreateNew = true;
    bEditorImport = false;
    bEditAfterNew = true;
}

UObject* UTransportationNetworkFactoryNew::FactoryCreateNew(
    UClass* InClass, UObject* InParent, FName InName,
    EObjectFlags InFlags, UObject* InContext,
    FFeedbackContext* InWarn)
{
    check(Class->IsChildOf(UTransportationNetwork::StaticClass()));
    return NewObject<UTransportationNetwork>(
	    InParent, InClass, InName, InFlags);
}

Bool UTransportationNetworkFactoryNew::ShouldShowInNewMenu() const
{
    return true;
}
```

