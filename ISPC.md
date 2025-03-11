If you get compiler errors for `*.ispc.generated.h` files then disable ISPC.

This is done from the project's `.Target.cs` file:
```csharp
bCompileISPC = false;
```

An alternative is to disable [[Unreal Build Accelerator]].
Not sure why this works.
This is done from the [[Build Configurations]] XML file `~/.config/Epic/UnrealBuildTool/BuildConfiguration.xml`:
```xml
<?xml version="1.0" encoding="utf-8" ?> 
<Configuration xmlns="https://www.unrealengine.com/BuildConfiguration">
    <BuildConfiguration>
        <bAllowUBAExecutor>false</bAllowUBAExecutor>
        <bAllowUBALocalExecutor>false</bAllowUBALocalExecutor>
    </BuildConfiguration>
</Configuration>
```

