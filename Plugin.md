Plugins are C++ code that Unreal Engine may load during start-up.
Plugins can either be installed to the engine or to the project.
Engine installed plugins are in `UE_ROOT/Engine/Plugins`.
Project installed plugins are in `PROJECT_ROOT/Plugins`.
Plugins can be enabled or disabled from Top Menu Bar > Edit > Plugins.
They can also be enabled or disabled from the project's `.uproject` file.
A [[Module]] provided by a Plugin can be referenced from another Module's `.Build.cs`,
through the `PublicDependencyModuleNames`, `PrivateDependencyModuleNames`, and `DynamicallyLoadedModuleNames` lists.

A plugin provides more or more [[Module|Modules]].