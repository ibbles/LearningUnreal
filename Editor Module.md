An editor [[Module]] is a collection of C++ code.
It is allowed to interface with Unreal Editor code.
It will not be part of packaged applications.
Do not build application logic in an editor module.

Modules are stored within the `Source` directory of the project or plugin it is part of.
In this example the project is named `MyProject`.
Each module has its own subdirectory in `Source` named after the module.
In this example we create a module named `MyProjectEditor`.
As with any other [[Module]], an Editor module has a `.Build.cs` file.
In this example it is called `MyProjectEditor.Build.cs`.
It is common to put source files within `Public` and `Private` subdirectories in `Source`.
We have the following file system structure:
```
MyProject\
	Source\
		MyProject\
			The soure code you already have.
		MyProjectEditor\
			Public\
			Private\
				MyProjectEditor.h
				MyProjectEditor.cpp
			MyProjectEditor.Build.cs
		MyProject.Target.cs
		MyProjectEditor.Target.cs
	MyProject.uproject
```

`MyProjectEditor.Build.cs`:
```c#
using UnrealBuildTool;

public class MyProjectEditor : ModuleRules
{
	public MyProjectEditor(ReadOnlyTargetRules Target) : base(Target)
	{
		bLegacyPublicIncludePaths = false;
		PCHUsage = PCHUsageMode.UseExplicitOrSharedPCHs;
		PrecompileForTargets = PrecompileTargetsType.Any;

		PublicDependencyModuleNames.AddRange(new string[]{
			"Core", "CoreUObject", "Engine"
		});

		PrivateDependencyModuleNames.AddRange(new string[] {
			"EditorStyle", "Slate", "SlateCore", "UnrealEd",
		});

#if UE_5_0_OR_LATER
		PrivateDependencyModuleNames.Add("EditorFramework");
#endif
	}
}
```

Which module dependencies are necessary varies from project to project.

The module must be listed in `MyProjectEditor.Target.cs`:
```c#
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


The module must be listed in the project's `.uproject` or plugin's `.uplugin` file:
`MyProject.uproject`:
```json
{
	// Other stuff here.
	
	"Modules": [
		{
			"Name": "MyProject",
			"Type": "Runtime",
			"LoadingPhase": "Default"
		},
		{
			"Name": "MyProjectEditor",
			"Type":, "Editor",
			"LoadingPhase": "Default"
		}
	],
	
	// Other stuff here.
}
```

A module comes with a header / source pair that implements the module interface.

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
    virtual void ShutdownModule() override
};
```

`MyProjectEditor.cpp`:
```cpp
// Project includes.
#include "MyProjectEditor.h"

void FMyProjectEditorModule::StartupModule()
{
}

void FMyProjectEditorModule::ShutdownModule()
{
}

IMPLEMENT_MODULE(FMyProjectEditorModule, MyProjectEditor);
```
