# Texture Streaming Pool # GB Over Budget

Unreal Engine allocates a limited amount of VRAM for texture streaming.
If we add too many different texture-using assets to our scene then we will fill that amount.
There is a [[CVars|CVar]] to control the size of the pool.
```
r.Streaming.PoolSize
```
This measures the size of the texture streaming pool in MiB.
By default it is 1024 (I think).
If you have VRAM to spare then you can increase it.
```
r.Streaming.PoolSize 3000
```


# DotNet SDK SSL connection

Affects Unreal Engine 5.0 and newer, as of early 2022, Linux distributions.
I think problem is that Unreal Engine knowingly and unintentionally used a system library and when the distribution updated it Unreal Engine could no longer find the old old. The fix below tells it where to find the new one.

```
$ ./GenerateProjectFiles.sh
# Snip.
Setting up bundled DotNet SDK
NuGet.targets(255,5): error : Unable to load the service index for source https://api.nuget.org/v3/index.json. [/home/zlare/src/UnrealEngine/Engine/Source/Programs/UnrealBuildTool/UnrealBuildTool.csproj]
NuGet.targets(255,5): error :   The SSL connection could not be established, see inner exception. [/home/zlare/src/UnrealEngine/Engine/Source/Programs/UnrealBuildTool/UnrealBuildTool.csproj]
NuGet.targets(255,5): error :   The remote certificate is invalid according to the validation procedure. [/home/zlare/src/UnrealEngine/Engine/Source/Programs/UnrealBuildTool/UnrealBuildTool.csproj]
GenerateProjectFiles ERROR: Failed to build UnrealBuildTool
```

To fix, install `libicu50`, I assume this is a system package, and define a pair of environment variables.
Maybe try without the install first, you may have the package, or a corresponding one, already.

```
export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
export SSL_CERT_DIR=/dev/null
```

https://discord.com/channels/187217643009212416/375022233875382274/985662642129620992


# Tamil text in dialog boxes

Install some combination of the following system packages, depending on which Linux distribution you are on.
- `xorg-fonts-misc`
- `xorg-x11-fonts`
- `xorg-x11-fonts-misc`

# GPU process launch failed

This is caused by the Chromium Embedded Framework, which is used for the Web Browser Plugin.
It can be disabled by passing `-NoCEF` when starting the Editor, and maybe also exported projects.

Example error message:
```
ERROR:gpu_process_host.cc(955)] GPU process launch failed: error_code=1002
WARNING:gpu_process_host.cc(1274)] The GPU process has crashed 8 time(s)
ERROR:gpu_process_host.cc(955)] GPU process launch failed: error_code=1002
WARNING:gpu_process_host.cc(1274)] The GPU process has crashed 9 time(s)
FATAL:gpu_data_manager_impl_private.cc(414)] GPU process isn't usable. Goodbye.
```


# ICU Version

```
Couldn't find a valid ICU package installed on the system.
```

This problem appeared with Unreal Engine 5.0, and was fixed in 5.1 by bundling the ICU.
This is a .NET thing.
That is needed when building C++ projects.
You can tell it to use a specific ICU version with
```bash
export DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=1
export DOTNET_SYSTEM_GLOBALIZATION_APPLOCALICU="69.1"
```

You can find out which version you have with
```bash
find /usr/lib/icu/