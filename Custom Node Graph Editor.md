See also [[Custom Node Graph Editor References]] for a summary of other sources on custom node graph editors.
See also [[Node Graph Editor]] for a summary of the classes involved in creating a node graph editor.
See also [[Sound Cue Node Graph Editor]] for a high-level description of how the engine built-in [[Sound Cue Editor]] has been implemented.

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


# Asset Creation

Creating a new asset type require at least three classes
- One for the asset itself, inherits from `UObject`.
	- `UTransportationNetwork` in our example.
- One that handles creation of new instances, inherits from `UFactory`.
	- `UTransportationNetworkFactoryNew` in our example.
- One that handles [[Content Browser]] integration, inherits from `FAssetTypeActions_Base`.
	- `FTransportationNetworkActions` in our example.


## `UTransportationNetwork`

This is our main [[Asset]] type.
It corresponds to the `USoundCue` type in [[Sound Cue Node Graph Editor]].
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
but it is not yet possible to create a Transportation Network instances for the reference to reference.
You can verify that Unreal Editor sees the new class using Top Menu Bar > Tools > Class Viewer, disable the Actor filter, and type the name of the asset class in the search box.


## `UTransportationNetworkFactoryNew`

A factory is responsible for creating and initializing new instances of the asset type.
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


## `FTransportationNetworkActions`

This class is what makes it possible for the user to create new instances of the asset from the [[Content Browser]].

There are two parts to this.
- Create a class inheriting from `FAssetTypeActions_Base`.
- Register the class with the `AssetTools` module.

`Source/MyProjectEditor/Public/TransportationNetworkActions.h`:
```cpp
#pragma once

// Unreal Engine includes.
#include "CoreMinimal.h"
#include "AssetTypeActions_Base.h"

class FTransportationNetworkActions
	: public FAssetTypeActions_Base
{
public:
    //~ Begin FAssetTypeActions Interface
	FText GetName() const override;
	UClass* GetSupportedClass() const override;
	FColor GetTypeColor() const override;
	uint32 GetCategories() override;
    //~ End FAssetTypeActions Interface
};
```

`Source/MyProjectEditor/Private/TransportationNetworkActions.cpp`:
```cpp
#include "TransportationNetworkActions.h"

// My Project includes.
#include "Transportation/TransportationNetwork.h"

FText FNormalFTransportationNetworkActions::GetName() const
{
    return TEXT("Transportation Network");
}

UClass* FNormalFTransportationNetworkActions::GetSupportedClass() const
{
    return UTransportationNetwork::StaticClass();
}

FColor FNormalFTransportationNetworkActions::GetTypeColor() const
{
    return FColor::Emerald;
}

uint32 FNormalFTransportationNetworkActions::GetCategories()
{
	// It is possible to create custom asset categories.
	// Out of scope for this example, and a limited resource
	// so don't do that unnecessarily.
    return EAssetTypeCategories::Misc;
}
```


The Asset Type Actions class is registered with the `AssetTools` module from the module interface class, in the `StartupModule` function.
And unregistered from the `ShutdownModule` functions.
In order to unregister the instance we need to store a reference to it in the module class.

```cpp
#pragma once

// Unreal Engine includes.
#include "CoreMinimal.h"
#include "Modules/ModuleInterface.h"

class FMyProjectEditorModule : public IModuleInterface
{
public:
    virtual void StartupModule() override;
    virtual void ShutdownModule() override;

private:
	TSharedPtr<IAssetTypeActions> AssetActions;
};
```

```cpp
void FUnrealTestBedEditorModule::ShutdownModule()
{
	AssetActions =
		MakeShareable(new FTransportationNetworkAction(Category))

	IAssetTools& AssetTools = IAssetTools::Get();
	AssetTools.RegisterAssetTypeActions(
		AssetAction.ToSharedRef());
}

void FUnrealTestBedEditorModule::ShutdownModule()
{
	IAssetTools& AssetTools = IAssetTools::Get();
	AssetTools.UnregisterAssetTypeActions(
		AssetActions.ToSharedRef());
}
```

It is now possible to right-click the [[Content Browser]] and find Transportation Network under Miscellaneous.
After creating an instance of the asset we can set the Blueprint variables of that type to point to the new asset instance.


# Network

Nodes in the network represent facilities.
They are the runtime representation of the node graph nodes.
They are implemented as subclasses of `UFacility`.
`UFacility` correspond to the `USoundNode` in [[Sound Cue Node Graph Editor]].
`UTransportationNetwork` has a list of these.

`Source/MyProject/Public/Facility.h`, partial:
```cpp
#pragma once

// Unreal Engine includes.
#include "UObject.h"

CLASS()
class MYPROJECT_API UFacility : public UObject
{
    GENERATED_BODY()
};
```


`Source/MyProject/Public/TransportationNetwork.h`, partial:
```cpp
class UFacility;

UCLASS()
class MYPROJECT_API UTransportationNetwork : public UObject
{
    UPROPERTY()
    TArray<UFacility*> Facilities;
};
```

At runtime the `UTransportationNetwork` and `

# References

- [_Creating a Custom Asset Type with its own Editor in C++_ by JanKXSKI @ dev.epicgames.com. July 7, 2022](https://dev.epicgames.com/community/learning/tutorials/vyKB/unreal-engine-creating-a-custom-asset-type-with-its-own-editor-in-c)

