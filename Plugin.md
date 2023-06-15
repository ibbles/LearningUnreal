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

A plugin provides one or more [[Module|Modules]].

A plugin is a piece of code or functionality that is optional.
Unreal Engine consists of the core engine and a large number of plugins.
Plugins can be enabled or disabled from Top Menu Bar > Edit > Plugins.
The plugins are separated into categories and whether they are installed to the current project or the engine.
Project plugins are stored in `$PROJECT_ROOT/Plugins`.
Engine plugins are stored in `$UE_ROOT/Engine/Plugins`.
Plugins we install ourselves are often placed in `$UE_ROOT/Engine/Plugins/Marketplace`.

A plugin can consist of [[Asset|Assets]], C++ code, or both.

A plugin with C++ code that is part of a project is built as part of regular project builds.
```shell
<PATH TO UE>/Engine/Build/BatchFiles/Linux/Build.sh \
	Linux \
	Development \
	-Project=<PATH TO PROJECTS>/<ProjectName>/<ProjectName>.uproject \
	-TargetType=Editor
```

If the plugin isn't built then check
- the `.uplugin` file for white/black or allow/deny lists for modules.
- the `.uplugin` file for supported target platforms.
- the `.uproject` file for a `Plugins` entry that disables the plugin.

Plugins can also be built explicitly.
```shell
<PATH TO UE>/Engine/Build/BatchFiles/RunUAT.sh \
	BuildPlugin \
	-Plugin=<path to .uplugin> \
	-Package=<output path for compiled plugin> \
 	-TargetPlatforms=Linux \
	-Rocket  
```

`-TargetPlatforms` is optional.

This is a plugin export, this is used when  a plugin is developed in one project and is to be used in another.
Then the `-Package` argument should be the path to the `Plugins/<PluginName>` directory in the user project.
On the corresponding directory in the engine installation.

