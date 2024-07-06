Modules are the building blocks that make up the engine, plugins, and projects.
A module is a unit of encapsulation, helps enforce code separation.
Used to keep different systems separated.
Such as inventory system, dialog system, and so on.
A Module is a collection of source files that are compiled together.
Each module becomes a shared library in editor builds.
Shipping builds compiles everything to a single binary.
Can be included or excluded independently.

Unreal Engine contains many modules, stored as subdirectories in `UE_ROOT/Engine/Source`.
The [[Plugin]] collection included in Unreal Engine also contains a bunch of modules, in subdirectories of `UE_ROOT_/Engine/Plugins/<PLUGIN_NAME>/Source`.

Modules are stored as directories within the `Source` directory of a project or plugin.
During building [[Unreal Build System]] will traverse the directory structure and compile the modules that need compiling.
Each module has a `<NAME>.Build.cs` file that is named based on the name of the module.
Describe how this module is to be built and what module dependencies it has.

The source code for a module is stored within the `Private` and `Public` subdirectories of the module subdirectory of the `Source` directory.


# Dependencies

Module can depend on each other, forming a directed acyclic graph.
Dependencies are listed in `PublicDependencyModuleNames`, `PrivateDependencyModuleName`, and `DynamicallyLoadedModuleNames`.
This give the current module access to the classes and other symbols in the dependee module.
Adding another module to a module's `.Build.cs` will cause that other modules' public include directories to be passed to the compiler when compiling this module, and the shared library for that module passed to the linker when linking this module.


# Primary Game Module

A game contains one module the is special: the primary game module.
The center point of the project, the module that makes use of all the other modules.
A game has only one such module and it is the one that has the same name as the project.
(
Is the name a requirement?
)

Module Initialization is slightly different for the primary game module compared to other modules.
Use the `IMPLEMENT_PRIMARY_GAME_MODULE` macro instead of `IMPLEMENT_MODULE`.
See _Module Initialization_ for details.


# Module Initialization

Most modules are initialized with the `IMPLEMENT_MODULE` macro.
Put the following into a `.cpp` file that has the same name as the module.
`MyModule.cpp`:
```cpp
#include "Modules/ModuleManager.h"
IMPLEMENT_MODULE(FDefaultGameModuleImpl, "MyModule")
```


The primary game module should instead use the `IMPLEMENT_PRIMARY_GAME_MODULE` macro.
`MyGame.cpp`:
```cpp
#include "Modules/ModuleManager.h"
IMPLEMENT_PRIMARY_GAME_MODULE(FDefaultGameModuleImpl, MyGame, "MyGame" );
```

This bit is generated for you when you create a new C++ project with Unreal Editor.

The corresponding header file, `MyModule.h` or `MyGame.h`, can be nothing more than
```cpp
#pragma once
#include "CoreMinimal.h"
```


If the module is not part of a game, i.e. it is part of a plugin or the engine itself, then use the `FDefaultModuleImpl` instead of `FDefaultGameModuleImpl`.


# Custom Module Behavior

A module can contain code that is run at certain points throughout the module's life cycle.
The `IMPLEMENT_MODULE` and `IMPLEMENT_PRIMARY_GAME_MODULE` macros, see _Module Initialization_, take a class name as parameter.
If we don't need to do anything custom then we can pass `FDefault(Game)?ModuleImpl` to `IMPLEMENT_MODULE` and `IMPLEMENT_PRIMARY_GAME_MODULE`.
If we do want to make some custom then we can declare a class that inherits from either `FDefaultGameModuleImpl` or `FDefaultModuleImpl` and pass that to the macro instead.

See the [`IModuleInterface`](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Core/Modules/IModuleInterface) class in `Engine/Source/Runtime/Core/Public/Modules/ModuleInterface.h`  for a list of virtual functions that your class can override.

An example module definition that prints a log message on module startup and shutdown:

`MyModule.h`:
```cpp
#pragma once

// Unreal Engine includes.
#include "CoreMinimal.h"
#include "Modules/ModuleManager.h"

class FMyModule : public FDefaultGameModuleImpl
{
public: // Member function overrides.
	//~ Begin IModuleInterface interface.
	virtual void StartupModule() override;
	virtual void ShutdownModule() override;
	//~ End IModuleInterface interface.
};
```

`MyModule.cpp`:
```cpp
#include "MyModule.h"

// Unreal Engine includes.
#include "Modules/ModuleManager.h"

void FMyModule::StartupModule()
{
	UE_LOG(LogTemp, Warning, TEXT("FMyModule::StartupModule live."));
}

void FMyModule::ShutdownModule()
{
	UE_LOG(LogTemp, Warning, TEXT("FMyModule::ShutdownModule live."));
}

IMPLEMENT_PRIMARY_GAME_MODULE(FMyModule, MyModule, "MyGame");
```

Beware that `ShutdownModule` is not called when closing Unreal Editor.
It is called when explicitly unloading a module from Top Menu Bar > Tools > Debug > Modules.


# Building A Module

A single module can be built with:
```shell
$UE_ROOT/Engine/Build/BatchFiles/Linux/Build.sh -Target="UnrealEditor Linux Development" -Module="MyModule"
```

or from within the Unreal Editor, Top Menu Bar > Tools > Debug > Modules > your module > click Recompile.
Here the module is listed with the name passed to `IMPLEMENT_MODULE` or `IMPLEMENT_PRIMARY_GAME_MODULE`,
not the class name.
Beware that recompiling a module while Unreal Editor is running comes with some risk.
See [_Don’t use hot reload in UE4!_](https://horugame.com/dont-use-hot-reload-in-ue4/)
(
I assume this also applies to Unreal Engine 5.
)

When the module is built from Unreal Editor it may break,
preventing Unreal Editor from starting again after closing it and building the project again from the command line with the following error message:
```
LogInit: Warning: Incompatible or missing module: MyModule

The following modules are miassing or built with a different engine version:
  MyModule
Would you like to rebuild them now?

[Yes] [No]
```

Clicking Yes doesn't help. It builds the module again but the error persists.
Even building again with
```cpp
➤ $UE_ROOT/Engine/Build/BatchFiles/Linux/Build.sh \
	Linux \
	Development \
	-Project=$PROJECT_ROOT/MyProject.uproject \
	-TargetType=Editor
```
isn't enough to fix it.
A nuke of the build artifacts is required:
```shell
# Big hammer:
➤ rm Intermediate/ -rf

# Slightly smaller hammer:
➤ rm Intermediate/Build/Linux/x64/MyProjectEditor/ -rf  
➤ rm Intermediate/Build/Linux/x64/UnrealEditor/ -rf
```
After that the project can be built with `Build.sh` and opened again.

# References

- [_The Unreal Build System Explained | Inside Unreal_ at 38:58 by Unreal Engine @ youtube.com](https://youtu.be/GJZUV8homoo?t=2338)
- [_Unreal Engine Modules_ by Epic Games @ docs.unrealengine.com](https://docs.unrealengine.com/5.0/en-US/unreal-engine-modules/)
- [_Creating a Gameplay Module_ by Epic Games @ docs.unrealengine.com](https://docs.unrealengine.com/5.2/en-US/how-to-make-a-gameplay-module-in-unreal-engine/)
