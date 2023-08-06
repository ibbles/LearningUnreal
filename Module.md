Modules are the building blocks that make up the engine, plugins, and projects.
A module is a unit of encapsulation, helps enforce code separation.
Used to keep different systems separated.
Such as inventory system, dialog system, and so on.
A Module is a collection of source files that are compiled together.
Each module become a shared library in editor builds.
Shipping builds compiles everything to a single binary.
Can be included or excluded independently.

Unreal Engine contains many modules, stored as subdirectories in `UE_ROOT/Engine/Source`.
The [[Plugin]] collection included in Unreal Engine also contains a bunch of modules, in subdirectories of `UE_ROOT_/Engine/Plugins/<PLUGIN_NAME>/Source`.

Modules are stored as directories within the `Source` directory of a project or plugin.
During building [[Unreal Build System]] will traverse the directory structure and compile the modules that need compiling.
Each module has a `<NAME>.Build.cs` file that is named based on the name of the module.
Describe how this module is to be build and what module dependencies it has.

The source code for a module is stored within the `Private` and `Public` subdirectories of the module subdirectory of the `Source` directory.


# Dependencies

Module can depend on each other, forming a directed acyclic graph.
Dependencies are listed in `PublicDependencyModuleNames`, `PrivateDependencyModuleName`, and `DynamicallyLoadedModuleNames`.
This give the current module access to the classes and other symbols in the dependee module.
Adding another module to a module's `.Build.cs` will cause that other modules' public include directories to be passed to the compiler when compiling this module, and the shared library for that module passed to the linker when linking this module.


# Primary Game Module

Has the same name as your project.
The center point of the project, the module that makes use of all the other modules.


# References

- [_The Unreal Build System Explained | Inside Unreal_ at 38:58 by Unreal Engine @ youtube.com](https://youtu.be/GJZUV8homoo?t=2338)
- [_Unreal Engine Modules_ by Epic Games @ docs.unrealengine.com](https://docs.unrealengine.com/5.0/en-US/unreal-engine-modules/)
- [_Creating a Gameplay Module_ by Epic Games @ docs.unrealengine.com](https://docs.unrealengine.com/5.2/en-US/how-to-make-a-gameplay-module-in-unreal-engine/)
