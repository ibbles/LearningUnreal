# Texture Streaming Pool # GB Over Budget

Unreal Engine allocates a limited amount of VRAM for texture streaming.
If we add too many different texture-using assets to our scene then we will fill that amount.
There is a [[Console Variable]] to control the size of the pool.
```
r.Streaming.PoolSize
```
This measures the size of the texture streaming pool in MiB.
By default it is 1024 (I think).
If you have VRAM to spare then you can increase it.
```
r.Streaming.PoolSize 3000
```


# The Total Lightmap Size Of This InstancedStaticMeshComponent Is Large

This can appear in the Message Log panel when [[Building Lighting]] for a level with foliage.
The [[Lightmap]] size for foliage is reduced in [[Foliage Mode]] > Foliage panel > select one or more [[Foliage Type]]s > Details > Instance Settings > Light Map Resolution.
By enabling the override checkbox and reducing the value we reduce the [[Lightmap]] size for our foliage instances.
This should fix the warning, possibly improve performance, and reduce memory usage.

[_Use Fix and optimize foliage in unreal_ - Light Map Resolution, by Batnobie X @ youtube.com. 2021](https://youtu.be/jcZ5V8qFwgE?t=100)


# Blurry Quixel Megascans 3D Assets

[[Static Mesh]]es imported from the Quixel Library sometimes become blurry in the [[Viewport]].
It looks like the textures aren't being loaded properly.
One possible cause is that a [[Texture]] that should not be [[Virtual Texture]] accidentally has become marked as such.
You can verify if this is the case by the presence of the VT decorator on the texture's icon in the [[Content Browser]].
To find the texture, open the [[Material]] used for the Quixel Megascans [[Static Mesh]].
In the Textures section of the parameter list, click the Browse To Asset button next to any texture, the folder opened in the Content Browser usually hold all textures.
If any of the textures has the VT decorator, then open it in the [[Texture Editor]] and disable Details panel > Texture > Virtual Texture Streaming.

[_Unreal Engine 5: Fix for blurry highres megascans textures (RVT) !CHECK PINNED COMMENT FOR UPDATE!_ by hcaq @ youtube.com 2022](https://www.youtube.com/watch?v=e19e6XKk6Wc)
-

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