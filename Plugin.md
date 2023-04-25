Plugins are C++ code that Unreal Engine may load during start-up.
Plugins can either be installed to the engine or to the project.
Engine installed plugins are in `UE_ROOT/Engine/Plugins`.
Manually installed plugins should be put in the `Marketplace` subdirectory.
(
Don't know why, or if this is really a requirement.
)
Project installed plugins are in `PROJECT_ROOT/Plugins`.
Plugins can be enabled or disabled from Top Menu Bar > Edit > Plugins.
They can also be enabled or disabled from the project's `.uproject` file.
```json
{
        "Plugins": [
                {
                        "Name": "MyPluginName",
                        "Enabled": true
                        "SupportedTargetPlatforms": [
                                "Win64",
                                "Linux"
                        ]
                }
        ]
}
```

A [[Module]] provided by a Plugin can be referenced from another Module's `.Build.cs`,
through the `PublicDependencyModuleNames`, `PrivateDependencyModuleNames`, and `DynamicallyLoadedModuleNames` lists.

A plugin provides more or more [[Module|Modules]].


A plugin is a piece of code or functionality that is optional.
Unreal Engine consists of the core engine and a large number of plugins.
Plugins can be enabled or disabled from Top Menu Bar > Edit > Plugins.
The plugins are separated into categories and whether they are installed to the current project or the engine.
Project plugins are stored in `$PROJECT_ROOT/Plugins`.
Engine plugins are stored in `$UE_ROOT/Engine/Plugins`.
Plugins we install ourselves are often placed in `$UE_ROOT/Engine/Plugins/Marketplace`.

A plugin can consist of [[Asset|Assets]], C++ code, or both.
