A custom asset is an [[Asset]] type that is part of a project or [[Plugin]].
Steps to create a new asset type:
- Declare the asset type's C++ class.
- Implement asset factories.
- Customize asset appearance in Content Browser.
- Customize asset editor UI.
- Add asset-specific Content Browser actions.


# Asset Class

The asset's C++ class define the content of the asset.
The class should inherit from `UObject`.
There may be other non-Unreal Engine classes between `UObject` and the asset class.
The class should be marked with the `*_API` macro for the module it is in.
The class should be marked with the `UCLASS` macro,
with the `BlueprintType` [[Class Specifier]] if the asset should be usable as a [[Blueprint Variable]] or in [[Blueprint Visual Script]].

Need the following headers: 
- `"CoreMinimal.h"`
- `"UObject/Object.h"`
- `"UObject/ObjectMacros.h"`.

As with all generated classes, also need `"<CLASS_NAME>.generated.h"` last in the include list.
Non-static data members that form the salient properties of the asset should be marked with `UPROPERTY`.

`Source/MyProject/Public/MyAsset.h`:
```cpp
#pragma once

#include "CoreMinimal.h"
#include "UObject/Object.h"
#incldue "UObject/ObjectMacros.h"
#include "MyAsset.generated.h"

UCLASS(BlueprintType)
class MYMODULE_API UMyAsset : public UObject
{
    GENERATED_BODY()

public:
    UPROPERTY(VisibleAnywhere, BlueprintReadOnly)
    double MyFloat;
};
```

At this point it is possible to build the project and create Blueprint Actors that have a variable that references `MyAsset`.
But it it not yet possible to create `MyAsset` instances in the [[Content Browser]] from within the editor.

One can bypass all the factory stuff described below by having the asset type derive from `UDataAsset` instead of `UObject`.
Then the asset type will show up in the Content Browser > right-click > Miscellaneous > Data Asset list.
By default these assets get the standard property editor, but can have a custom editor, i.e a [[Details Customization]].


# Factory

A Factory is used when creating instances of the asset type.
Factories are editor concepts and should be in an [[Editor Module]].
Factories are C++ classes that inherit from `UFactory`.
`UFactory` provides virtual functions to be overridden.
The main forms of integrations with the editor are already implemented by `UFactory`,
need only provide the asset-specific details in the virtual functions.
There are three types of factories:
- `<ASSET_NAME>FactoryNew`: When creating asset from the Content Browser right-click menu.
- `<ASSET_NAME>Factory`: When a file is being drag-and-dropped into the Content Browser.
- `Reimport<ASSET_NAME>Factory`: Recreate assets when files on disk changed. Can be part of `<ASSET_NAME>Factory` instead.

Three types only for historical reasons. May be modernized in the future so that only a single factory class is required.

Example `MyAssetFactoryNew`, a factory that creates `MyAsset` instances from the Content Browser right-click menu.

`Source/MyProjectEditor/Public/MyAssetFactoryNew.h`:
```cpp
#pragma once

// Unreal Engine includes.
#include "Factories/Factory.h"

#include "MyAssetFactoryNew.generated.h"

UCLASS()
class MYEDITORMODULE_API UMyAssetFactoryNew : public UFactory
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
}
```

`Source/MyProjectEditor/Private/MyAssetFactoryNew.cpp`:
```cpp
#include "MyAssetFactoryNew.h"

UMyAssetFactoryNew::UMyAssetFactoryNew(
	const FObjectInitializer& ObjectInitializer)
    : Super(ObjectInitializer)
{
    // Tell the editor which type of assets this factor can
    // create.
    SupportedClass = UMyAsset::StaticClass();

    // This factory creates new instances from scratch
    // rather than importing using drag-and-drop. This is
    // what decide which of the three factory types this
    // factory is.
    bCreateNew = true;
    bEditorImport = false;
    
    // Open the asset editor when a new asset is created.
    bEditAfterNew = true;
}

// Called by the engine when a new instance of the asset
// type is to be created. That is, when the  user has
// right-clicked in the Content Browser and selected MyAsset
// from the list.
UObject* UMyAssetFactoryNew::FactoryCreateNew(
	UClass* InClass, UObject* InParent, FName InName,
	EObjectFlags InFlags, UObject* InContext,
	FFeedbackContext* Warn)
{
    check(Class->IsChildOf(UMyAsset::StaticClass()));
    return NewObject<UMyAsset>(
	    InParent, InClass, InName, InFlags);
}

// Return true to make the MyAsset asset show up in the Content Browser context menu.
bool UMyAssetFactoryNew::ShouldShowInNewMenu() const
{
    return true;
}
```


There is also `UActorFactory`, which is used when an asset is dragged from the Content Browser into the [[Level Viewport]].

There is also `IComponentAssetBroker`, which is used when an asset is dragged from the Content Browser into the Components Panel of a Blueprint class.

To get the Asset to show up in the [[Content Browser]] right-click [[Asset]] creation menu we need to create an Editor Customization in the form of a `FAssetTypeActions_Base`.


# Editor Customization

## Asset Type Actions

Create a class that inherties from `FAssetTypeActions_Base`.


## Asset Customization

The asset can have custom color, text, and icon in the Content Browser.
The icon can be any UI widget, including a 3D viewport.
Look into `UThumbnailRenderer`.

Asset Action are used to customize the look and feel of assets in the Editor, and to provide custom actions that can be performed on the assets.
Custom editor actions on assets are created by inheriting from `FAssetTypeActions_Base`.
`FAssetTypeActions_Base` is a helper class, one can also inherit from the `IAssetTypeActions` interface.
The actual action is registered/implemented in `IAssetTypeActions::GetActions`.
The implementation can be a lambda callback.
The action must be registered with the Editor.
This is often done in the module's `IModuleInterface` implementation, which is in `<MODULE_NAME>Module.cpp`.

To register an asset tool, load the `AssetTools` module and call `RegisterAssetTypeAction`.
The `AssetTools` module maintains a registry of of all the asset tools.
Use `RegisterAssetTypeAction`, passing in the `AssetTools` and a new instance of the `IAssetTypeActions` subclass.
```c++
virtual void StartupModule() override
{
    Style = MakeShareable(new FMyAssetEditorStyle());
    IAssetTools& AssetTools = 
        ModuleManager::LoadModuleChecked<FAssetToolsModule>(
            "AssetTools").Get();
    RegisterAssetTypeActions(
        AssetTools, MakeShareable(new FMyAssetActions(Style.ToSharedRef())));
}
```
Not sure what the `Style` is.
Not sure where in the Unreal Editor UI this shows up.

Another variant is to register an Advanced Asset Category.
Then a collection of Asset Actions can be collected in a submenu of the Content Browser context menu.
```cpp
virtual void StartupModule() override
{
    IAssetTools& AssetTools =
        FModuleManager::LoadModuleChecked<FAssetToolsModule>("AssetTools").Get();

    EAssetTypeCategories::Type MyAssetCategoryBit =
        AssetTools.RegisterAdvancedAssetCategory(
            FName(TEXT("MyCategoryName")),
            LOCTEXT("MyCategoryLabel", "MyCategoryLabel"));

    // Repeat for each IAssetTypeActions class.
    TSharedPtr<IAssetTypeActions> MyAssetActions =
        MakeShareable(new FMyAssetActions(MyAssetCategoryBit));
    AssetTools.RegisterAssetTypeActions(MyAssetActions.ToSharedRef());
}
```

By default the asset editor is generated automatically in the same way as the Details Panel.
This can be customized.
The Blueprint and Material editors are examples of custom asset editors.

We need two classes:
- `FMyAssetActions : IAssetTypeActions`: To create the new editor when needed.
- `FMyAssetEditor : FAssedEditorToolkit, FEditorUndoClient, FGCObject`: The asset editor.
Both classes should be in the Editor module of the plugin

To provide a custom asset editor override `OpenAssetEditor` in your `IAssetTypeActions` subclass.
"Toolkit" is a synonym for Asset Editor. 
Toolkit often used within the code, for type names and such.
Asset Editor is often used within documentation and articles.
```
void OpenAssetEditor(
    const TArray<UObject*>& InObjects,
    TSharedPtr<IToolkitHost> EditWithinLevelEditor)
```
- `InObjects`: The objects being edited. Can be multiple since multiple objects can be selected in the Content Browser. Loop over these and use `Cast` to determine if it's a type of object supported .
- `EditWithinLevelEditor`: Passed on to the `EditorToolkit` instances we create.

For each object, create and initialize a new `FMyAssetEditorToolkit`:
```
TSharedRef<FTextAssetEditorToolkit> EditorToolkit = MakeShareable(
    new FMyAssetEditorToolkit(Style));

EditorToolkit->Initialize(MyAsset, Mode, EditWithinLevelEditor);
```

`Style` is the one that was created in `StartupModule`.
`MyAsset` is the `Cast`ed object we got form `InObjects`.
`Mode` either `WorldCentric` or `Standalone` depending on if `EditWithinLevelEditor` is valid.

Full example:
```
void FMyAssetActions: OpenAssetEditor(
    const TArray<UObject*>& InObjects,
    TSharedPtr<IToolkitHost> EditWithinLevelEditor)
{
    EToolkitMode::Type Mode = EditWithinLevelEditor.IsValid()
        ? EToolkiMode::WorldCentric
        : EToolkitMode::Standalone;
    for (auto ObjIt = InObjects.CreateConstIterator(); ObjIt; ++ObjIt)
    {
        UMyAsset* MyAsset = Cast<UMyAsset(*ObjIt);
        if (MyAsset == nullptr)
        {
            continue;
        }
        TSharedRef<FTextAssetEditorToolkit> EditorToolkit = MakeShareable(
            new FMyAssetEditorToolkit(Style));
        EditorToolkit->Initialize(MyAsset, Mode, EditWithinLevelEditor);
    }
}
```

The call to `Initialize` will cause the asset editor to show up.

The `FMyAssetEditor` class is much larger. Too large to describe in detail here.
See [TextAsset@github.com](https://github.com/ue4plugins/TextAsset) for the full code.
In short, the editor is built inside a `Layout` where a big expression is used to create the splits that the editor window is divided into.
This is where the layout of the tabs is declared.

A collection of tabs that share an area on the screen is called a tab well.
With the Layout created we call `FAssetEditorToolkit::InitAssetEditor`.

You can find examples of how to create Styles in  `Engine/Source/Editor/EditorStyle/Private/SlateEditorStyle.cpp`.


[[Module]]
[[Callback]]
[[Garbage collection]]

[[Asset]]
[[Asset Loading]]
[[Naming Convention]]
[[UPROPERTY]]


# References
 
- [_Creating a Custom Asset Type with its own Editor in C++_ by JanKXSKI @ dev.epicgames.com. July 7, 2022](https://dev.epicgames.com/community/learning/tutorials/vyKB/unreal-engine-creating-a-custom-asset-type-with-its-own-editor-in-c)
- [Custom Asset Editors Code Exploration @ learn.unrealengine.com](https://learn.unrealengine.com/course/2436528/module/5372752?moduletoken=UHxxnDLPW8ROFOnyLDf7jkpDsWH-6EgInZkaEVy-utqnxQVniUN~z2b6Dq5f9wUr&LPId=0)
- [TextAsset @ github.com](https://github.com/ue4plugins/TextAsset)  
- [C++ Extending the Editor | Live Training | Unreal Engine by Unreal Engine @ youtube.com](https://youtu.be/zg_VstBxDi8?t=1731)
- [Writing a Custom Asset Editor for Unreal Engine 4](http://cairansteverink.nl/cairansteverink/blog/writing-a-custom-asset-editor-for-unreal-engine-4-part-1/)  
- [Asset Editor - Development of asset editor in Unreal Engine](https://easycomplex-tech.com/blog/Unreal-AssetEditor/)  
- [How to make tools in UE4](https://lxjk.github.io/2019/10/01/How-to-Make-Tools-in-U-E.html)  
