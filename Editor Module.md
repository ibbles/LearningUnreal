An editor [[Module]] is a collection of C++ code that extend the Unreal Editor.
It is allowed to call Unreal Editor functions and use Unreal Editor types.
It will not be part of packaged applications.
Do not build application logic in an editor module.

An editor module can, for example
- Customize the [[Details Panel]] for your [[Actor|Actors]], [[Component|Components]], and [[USTRUCT|`structs`]].
- Add new [[Editor Mode|Editor Modes]].
- Add [[Component Visualizer|Component Visualizers]].

Modules are stored within the `Source` directory of the project or plugin it is part of.
In this example the project is named `MyProject`.
Each module has its own subdirectory in the `Source` directory named after the module.
In this example we create a module named `MyProjectEditor`.
You can call it whatever you want.
It is possible to have multiple editor modules in a project or plugin.
The example editor module's  directory is `MyProject/Source/MyProjectEditor`.

As with any other [[Module]], an Editor module has a `.Build.cs` file.
In this example it is called `MyProjectEditor.Build.cs`.
A module mush have initialization and tear-down code.
This copy is typically stored in file named after the module.
`MyProjectEditor.(h|cpp)` in this case.
It is common, but not necessary, to put source files within `Public` and `Private` subdirectories.

We have the following file system structure:
```
MyProject\
	Source\
		MyProject\
			The main game module, with the soure code you already have.
		MyProjectEditor\
			Public\
				Public header files goes here.
			Private\
				Private header and implementation files goes here.
				MyProjectEditor.h
				MyProjectEditor.cpp
			MyProjectEditor.Build.cs
		MyProject.Target.cs
		MyProjectEditor.Target.cs
	MyProject.uproject
```

`MyProjectEditor.Build.cs`:
```csharp
using UnrealBuildTool;

public class MyProjectEditor : ModuleRules
{
	public MyProjectEditor(ReadOnlyTargetRules Target) : base(Target)
	{
		// See UE_ROOT/Engien/Source/Programs/UnrealBuildTool/Configuration/ModuleRules.cs
		// Reduce the number of directories to search for include files in.
		// Must include subdirectories of Public in #include directives.
		// May not be necessary, i.e. is the default, since Unreal Engine 5.0.
		bLegacyPublicIncludePaths = false;

		// See PCHUsageMode in UE_ROOT/Engien/Source/Programs/UnrealBuildTool/Configuration/ModuleRules.cs
		// Not sure what is best to put here, or if it can be left out.
		PCHUsage = PCHUsageMode.UseExplicitOrSharedPCHs;

		// See PrecompileTargetsType in UE_ROOT/Engien/Source/Programs/UnrealBuildTool/Configuration/ModuleRules.cs
		// Not sure what is best to p ut here, or if it can be left out.
		PrecompileForTargets = PrecompileTargetsType.Any;

		PublicDependencyModuleNames.AddRange(new string[]{
			"Core", "CoreUObject", "Engine", "MyProject"
		});

		PrivateDependencyModuleNames.AddRange(new string[] {
			"EditorStyle", "Slate", "SlateCore", "UnrealEd"
		});

#if UE_5_0_OR_LATER
		PrivateDependencyModuleNames.Add("EditorFramework");
#endif
	}
}
```

Which module dependencies are necessary varies from project to project.
A sign that one is missing is linker errors when building the project.
The documentation at [docs.unrealengine.com](docs.unrealengine.com) for classes and such name the module to add to the dependency list near the top of the page.

The module must be listed in `MyProjectEditor.Target.cs`:
```csharp
using UnrealBuildTool;
using System.Collections.Generic;

public class MyProjectEditorTarget : TargetRules
{
	public MyProjectEditorTarget(TargetInfo Target) : base(Target)
	{
		Type = TargetType.Editor;
		DefaultBuildSettings = BuildSettingsVersion.V2;
		ExtraModuleNames.AddRange( new string[] {
			"MyProject", "MyProjectEditor"
		});
	}
}
```

It should not be listed in `MyProject.Target.cs`, only the `-Editor` one.
(
[_Creating an Editor Module_ @ unrealcommunity.wiki. UE 4.26](https://unrealcommunity.wiki/creating-an-editor-module-x64nt5g3) talks about `if (Target.Type == TargetType.Editor)` in `MyProject.Target.cs`.
I get C# compiler error when I do that on Unreal Engine 5.1.
> `TargetInfo` does not contain a definition for `Type` and no accessible extension method `Type` accepting a first argument of type `TargetInfo` could be found
)

The module must be listed in the project's `.uproject` or plugin's `.uplugin` file:
`MyProject.uproject`:
```json
{
	// Other stuff here.
	
	// The Modules attribute lists all the modules that the
	// project contains. There are a number of settings that
	// can be set for each module. The three most common are
	// - Name: The name of the module and the directory where
	//         Unreal Build Tool will look for source files.
	// - Type: Most often either Runtime (for game logic), or
	//         Editor (for editor customization and extension).
	// - LoadingPhase:
	//         When this module should be loaded. Default work
	//         most of the time, but if the module have
	//         dependencies on engine modules then this may
	//         need to be customized, for example PostEngineInit.
	"Modules": [
		// This is the main project module, usually created by
		// the project setup wizard.
		{
			"Name": "MyProject",
			"Type": "Runtime",
			"LoadingPhase": "Default"
		},

		// This is the new editor module that we are adding
		{
			"Name": "MyProjectEditor",
			"Type":, "Editor",
			"LoadingPhase": "Default"
		}
	],
	
	// Other stuff here.
}
```

For more information on `LoadingPhase` see [_ELoadingPhase::Type_ @ docs.unrealengine.com](https://docs.unrealengine.com/5.1/en-US/API/Runtime/Projects/ELoadingPhase__Type/).


A module comes with a header / source pair that implements the module interface.
We should override at least `StartupModule` and `ShutdownModule`.
For additional virtual functions see [_`IModuleInterface`_ @ docs.unrealengine.com](https://docs.unrealengine.com/API/Runtime/Core/Modules/IModuleInterface/).
Once we add [[Details Customization|Details Customization]], [[Asset Type Action|Asset Type Actions]], [[Component Visualizer|Component Visualizers]], [[Placement Category|Placement Categories]], and [[Editor Mode|Editor Modes]] they will be registered in this class.

`MyProjectEditor.h`:
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
};
```

`MyProjectEditor.cpp`:
```cpp
// Project includes.
#include "MyProjectEditor.h"

void FMyProjectEditorModule::StartupModule()
{
	// Register Details Customizations and such here.
}

void FMyProjectEditorModule::ShutdownModule()
{
	// Unregister Details Customizations and such here.
}

// Tell Unreal Engine that the class FMyProjectEditorModule,
// first parameter, is the Module Interface for the module
// MyProjectEditor, second parameter.
IMPLEMENT_MODULE(FMyProjectEditorModule, MyProjectEditor);
```

A build of the project should build the new module
```shell
$ UE_ROOT/Engine/Build/BatchFiles/Linux/Build.sh \
	Linux \
	Development \
	-Project=PROJECT_ROOT/PROJECT.uproject \
	-TargetType=Editor
# ...
Building MyProjectEditor...
Building 2 actions with 2 processes...
[1/2] Compile MyProjectEditor.cpp
[2/2] Link (lld) libUnrealEditor-MyProjectEditor.so
# ...
```


# References

- [_Creating an Editor Module_ @ unrealcommunity.wiki. UE 4.26](https://unrealcommunity.wiki/creating-an-editor-module-x64nt5g3)
- [_Unreal Engine Modules_ by  Epic Games @ docs.unrealengine.com](https://docs.unrealengine.com/ProgrammingAndScripting/ProgrammingWithCPP/Modules/)
- [_Creating a Gameplay Module_ by Epic Games @ docs.unrealengine.com](https://docs.unrealengine.com/ProgrammingAndScripting/ProgrammingWithCPP/ModuleQuickStart/)
- [_Module Properties_ by Epic Games @ docs.unrealengine.com](https://docs.unrealengine.com/ProductionPipelines/BuildTools/UnrealBuildTool/ModuleFiles/)
