Debug(Game|Editor?), Development, Shipping.

The following is based on this [Unreal Source Discord discussion](https://discord.com/channels/187217643009212416/375022233875382274/1274458435634729111).

There is `~/.config/Unreal Engine/UnrealBuildTool/BuildConfiguration.xml` with some build configuration settings.
In some cases it may also use `~/UnrealEngine/Engine/Saved/UnrealBuildTool/`, not sure.

Mine currently has
```xml
<?xml version="1.0" encoding="utf-8" ?>
<Configuration xmlns="https://www.unrealengine.com/BuildConfiguration">
</Configuration>
```

We can set a project file generator.
```xml
<?xml version="1.0" encoding="utf-8" ?>
<Configuration xmlns="https://www.unrealengine.com/BuildConfiguration">
 <ProjectFileGenerator>
   <Format>Make</Format>
 </ProjectFileGenerator>
</Configuration>
```

Use `+` to have multiple generators:
```xml
<Format>Make+Rider</Format>
```

I've seen suggestion to make it
```xml
<BuildConfiguration>
	<bTuneDebugInfoForLLDB>true</bTuneDebugInfoForLLDB>
	<bDisableDumpSyms>true</bDisableDumpSyms>
</BuildConfiguration>
```

for debugging with LDDB with
```
settings set symbols.load-on-demand true
settings set target.preload-symbols false
settings set plugin.jit-loader.gdb.enable off
```
in `~/.lldbinit`.

See [[Debug C++ Code]].

Also consider
```cs
bOverrideBuildEnvironment = true;            // added for Rider
AdditionalCompilerArguments = " -glldb ";    // added for Rider (better formated compile errors)
```

in `.Builds.cs`.
Not sure why, or when we would want to do that.

If you are having compiler errors on generated header files, `.generated.h`, and you are on Unreal Engine 5.5, which introduced [[ISPC]], then try disabling [[Unreal Build Accelerator]], UBA.

```xml
<?xml version="1.0" encoding="utf-8" ?> 
<Configuration xmlns="https://www.unrealengine.com/BuildConfiguration">
    <BuildConfiguration>
        <bAllowUBAExecutor>false</bAllowUBAExecutor>
        <bAllowUBALocalExecutor>false</bAllowUBALocalExecutor>
    </BuildConfiguration>
</Configuration>
```

# References

- [_Rider Unreal Engine_ by JetBrains @ blog.jetbrains.com](https://blog.jetbrains.com/dotnet/2021/12/16/rider-unreal-engine-linux/)
