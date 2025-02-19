UBA for short.
Introduced with Unreal Engine 5.4 [(1)](https://www.unrealengine.com/en-US/blog/unreal-engine-5-4-is-now-available).

Is a distributed compilation driver.
Used by Unreal Build Tool.

Can be disabled using a [[Build Configurations]] setting:
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

- 1: [_Unreal Engine 5.4 is now available_ by Epic Games @ unrealengine.com/blog 2024](https://www.unrealengine.com/en-US/blog/unreal-engine-5-4-is-now-available)
- 2: [_Horde Unreal Build Accelerator and Remote Compilation Tutorial_ by Epic Games @ dev.epicgames.com/documentation 2024](https://dev.epicgames.com/documentation/en-us/unreal-engine/horde-unreal-build-accelerator-and-remote-compilation-tutorial-for-unreal-engine)
