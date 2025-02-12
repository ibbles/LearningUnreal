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