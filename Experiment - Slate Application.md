This note contains my attempts to build a Slate application.


There is a Slate App template on GitHub, [_UnrealSlateAppTemplate_](https://github.com/lpestl/UnrealSlateAppTemplate),  that demonstrates how to create a Slate application.
```shell
/media/s1300/unreal_projects
➤ git clone git@github.com:lpestl/UnrealSlateAppTemplate.git

# The .uproject file didn't have Engine Association set, so I added
#  "EngineAssociation": "5.4",
# to the file.

/media/s1300/unreal_projects/UnrealSlateAppTemplate  
⎇  main➤ /media/s1300/unreal_engine/5.4.2/Engine/Build/BatchFiles/Linux/Build.sh \
	Linux \
	Development \
	-Project=/media/s1300/unreal_projects/UnrealSlateAppTemplate/UnrealSlateAppTemplate.uproject \
	-TargetType=Editor
# Snip.
Targets with a unique build environment cannot be built with an installed engine.
```

Why not?

https://forums.unrealengine.com/t/targets-with-a-unique-build-environment-cannot-be-built-with-an-installed-engine/1353217 suggests two possible solutions:
- Remove `BuildEnvironment = TargetBuildEnvironment.Unique;` from the `.Taget.cs` files.
- Delete the `Editor.Target.cs` file.

In this case, there is no `BuildEnvironment` at all in any `.Target.cs`, and there is no `Editor.Target.cs`.
I have seen this error before.
At that time I fixed it by adding `DefaultBuildSettings = BuildSettingsVersion.Latest;` to `.Target.cs`.
Trying that.
No, that has already been set.

https://stackoverflow.com/questions/71535060/how-do-i-set-the-program-type hints that the problem is the `Type = TargetType.Program` that is the problem.
Suggests changing it to `TargetType.Game`.
Testing.

```shell
/media/s1300/unreal_projects/UnrealSlateAppTemplate
⎇  main➤ /media/s1300/unreal_engine/5.4.2/Engine/Build/BatchFiles/Linux/Build.sh \
	Linux \
	Development \
	-Project=/media/s1300/unreal_projects/UnrealSlateAppTemplate/UnrealSlateAppTemplate.uproject \
	-TargetType=Editor

Could not find definition for module 'EmptyProject', (referenced via UnrealEditor.Target.cs)
```

Let's not build `-TargetType=Editor`.
```shell
/media/s1300/unreal_projects/UnrealSlateAppTemplate
⎇  main➤ /media/s1300/unreal_engine/5.4.2/Engine/Build/BatchFiles/Linux/Build.sh \
	Linux  \
	Development \
	-Project=/media/s1300/unreal_projects/UnrealSlateAppTemplate/UnrealSlateAppTemplate.uproject \
	-TargetType=Game
# Snip.
UnrealSlateAppTemplate modifies the values of properties:
[
	bCompileAgainstEngine
].
This is not allowed, as UnrealSlateAppTemplate has build products in common with UnrealGame.
- Remove the modified setting,
- change UnrealSlateAppTemplate to use a unique build environment by setting 'BuildEnvironment = TargetBuildEnvironment.Unique;' in the UnrealSlateAppTemplateTarget constructor,
- or
- set bOverrideBuildEnvironment = true to force this setting on.
```

The `.Target.cs` file does indeed set `bCompileAgainstEngine` to false.
Let's not do that, commenting that line out.

```shell
/media/s1300/unreal_projects/UnrealSlateAppTemplate
⎇  main➤ /media/s1300/unreal_engine/5.4.2/Engine/Build/BatchFiles/Linux/Build.sh \
	Linux \
	Development \
	-Project=/media/s1300/unreal_projects/UnrealSlateAppTemplate/UnrealSlateAppTemplate.uproject \
	-TargetType=Game
# Snip.
Could not find definition for module 'EmptyProject', (referenced via UnrealSlateAppTemplate.Target.cs)
```

`EmptyProject` is the name of the project's main module.
Declared as follows in the `.uproject` file:
```js
	"Modules": [
		{
			"Name": "EmptyProject",
			"Type": "Runtime",
			"LoadingPhase": "Default"
		}
	]
```

But there is no directory in the `Source` directory with this name.
There is a folder named `UnrealSlateAppTemplate`.
Changing the `.uproject` `"name"` attribute to that.

```shell
/media/s1300/unreal_projects/UnrealSlateAppTemplate  
⎇  main➤ /media/s1300/unreal_engine/5.4.2/Engine/Build/BatchFiles/Linux/Build.sh \
	Linux  \
	Development \
	-Project=/media/s1300/unreal_projects/UnrealSlateAppTemplate/UnrealSlateAppTemplate.uproject \
	-TargetType=Game
# Snip.
Missing precompiled manifest for 'StandaloneRenderer', '/media/s1300/unreal_engine/5.4.2/Engine/Intermediate/Build/Linux/UnrealGame/Development/StandaloneRenderer/StandaloneRenderer.precompiled'. This module was most likely not flagged for being included in a precompiled build - set 'PrecompileForTargets = PrecompileTargetsType.Any;' in StandaloneRenderer.build.cs to override. If part of a plugin, also check if its 'Type' is correct.
```

OK.
What is a "precompiled manifets" and should "StandaloneRenderer" have one or not?
It suggests editing the engine installation, but I'm not going to do that.

Is this because the project's `.Target.cs` sets `LinkType = TargetLinkType.Monolithic;`?
Commenting that line out and trying again.

```shell
/media/s1300/unreal_projects/UnrealSlateAppTemplate[6]
⎇  main➤ /media/s1300/unreal_engine/5.4.2/Engine/Build/BatchFiles/Linux/Build.sh \
	Linux \
	Development \
	-Project=/media/s1300/unreal_projects/UnrealSlateAppTemplate/UnrealSlateAppTemplate.uproject \
	-TargetType=Game
# Snip
Missing precompiled manifest for 'StandaloneRenderer', '/media/s1300/unreal_engine/5.4.2/Engine/Intermediate/Build/Linux/UnrealGame/Development/StandaloneRenderer/StandaloneRenderer.precompiled'. This module was most likely not flagged for being included in a precompiled build - set 'PrecompileForTargets = PrecompileTargetsType.Any;' in StandaloneRenderer.build.cs to override. If part of a plugin, also check if its 'Type' is correct.
```

Same error.

Maybe the Requirements list in the template [README](https://github.com/lpestl/UnrealSlateAppTemplate?tab=readme-ov-file#requirements) is correct in that a from-sources build of Unreal Engine is required.
That would be disappointing.

So.
Maybe we will never be able to build this application, Unreal Engine just isn't built to support that.
I can still read the code though.
Maybe I'll learn something from that, enough to replicate it in a "regular" (whatever that means) Unreal Engine project.


# References

- [_UnrealSlateAppTemplate_ by Michael S. Kataev @ github.com 2024](https://github.com/lpestl/UnrealSlateAppTemplate)
- [_Slate UI Framework_ by Epic Games @ dev.epicgames.com/documentation](https://dev.epicgames.com/documentation/en-us/unreal-engine/slate-user-interface-programming-framework-for-unreal-engine)
- [_Making UIs With C++ in Unreal Engine, by Ben UI_ by JetBrains @ youtube.com 2022](https://www.youtube.com/watch?v=1n3oIfI7nBM)
- [_Unreal UIs and C++: Slate_ by ben ui](https://benui.ca/unreal/ui-cpp-slate/)

