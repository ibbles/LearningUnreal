# Failed to load Vulkan Driver which is required to run the engine.

This can have many causes.
For Unreal Engine 5.5.4 and currently, as of this writing, unreleased 5.6 require `VK_EXT_mesh_shader`.
https://forums.unrealengine.com/t/unreal-engine-5-6-startup-vulkan-error-and-vk-ext-mesh-shader/2332700/2

# Compiler Error On Generated Header Files `.generated.h`

Try disabling [[Unreal Build Accelerator]] using a [[Build Configurations]].
```xml
<?xml version="1.0" encoding="utf-8" ?> 
<Configuration xmlns="https://www.unrealengine.com/BuildConfiguration">
    <BuildConfiguration>
        <bAllowUBAExecutor>false</bAllowUBAExecutor>
        <bAllowUBALocalExecutor>false</bAllowUBALocalExecutor>
    </BuildConfiguration>
</Configuration>
```

# Rider Or Any IDE Not Finding Project Header Files In Public Directory

Add `bLegacyPublicIncludePaths = false;` to the module's `.Build.cs`.


# Cannot create a Vulkan device. Try updating your video driver to a more recent version

[Reported](https://discord.com/channels/187217643009212416/375022233875382274/1073323950651756745) on Ubuntu 22.04 with an NVIDIA GPU and the 525 driver when launching Unreal Engine 5.1.1 for the first time.
One suggestion is to install the `libvulkan1` package, which contains the Vulkan loader library.
One user report that reinstalling it fixed the problem. Unsure what "it" refers to.


# Crash When Starting Dedicated Server

CEF - Chromium Embedded Framework

Unreal Engine 5.0, maybe also 5.1.

Crash when launching a dedicated server.
```
CEF GPU acceleration enabled
The GPU process has crashed
Network service crashed, restarting service.
```

Try passing `-nocef` when starting the server.

https://discord.com/channels/187217643009212416/375022233875382274/1024835595865964574
Also
https://discord.com/channels/187217643009212416/375022233875382274/1025171533863342110
for a possible (complicated) workaround for 5.0.3.

The problem may be that files aren't copied when the server is packaged.
Compare
https://github.com/EpicGames/UnrealEngine/blob/5.0.3-release/Engine/Source/ThirdParty/CEF3/CEF3.build.cs#L63-L68
end
https://github.com/EpicGames/UnrealEngine/blob/5.0.3-release/Engine/Source/ThirdParty/CEF3/CEF3.build.cs#L127-L130

If the crash also happens when playing in the editor then the problem is something else.


# No suitable version of `libssl` was found

Happens when generating project files, either through `GenerateProjectFiles.sh`, or when creating a new C++ project.
I assume the latter uses the former.
Error that happens on Ubuntu 22.04 because system `libssl` was updated.
It happens even with official 5.0.3 binaries downloaded from https://www.unrealengine.com/en-US/linux.
Discussed in the Unreal Slackers Discord Linux channel at https://discord.com/channels/187217643009212416/375022233875382274/1009436112084811816.
Known by Epic Games, but not fixed in time for 5.0.3. 5.1 should be fine.
The solution may be to install an older version of `libssl` manually.
```
wget http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.0g-2ubuntu4_amd64.deb
sudo dpkg -i libssl1.1_1.1.0g-2ubuntu4_amd64.deb
```

Similar suggestion for Fedora from https://forums.unrealengine.com/t/generateprojectfiles-sh-fails-when-building-from-source-on-fedora-36-linux/519324 suggests we install `compat-openssl10` with `dnf`.


This is a DotNet problem, and there is a related GitLab issue for that project as well: https://github.com/dotnet/core/issues/4749
Suggest we download,  build and install `libssl 1.1.1` ourselves.
```
wget https://www.openssl.org/source/openssl-1.1.1c.tar.gz
tar -xzvf openssl-1.1.1c.tar.gz
cd openssl-1.1.1c
./config
make
sudo make install # It puts it into /usr/local/lib so it doesn't mess with the rest of your system)
LD_LIBRARY_PATH="/usr/local/lib" dotnet
```


# Ionic.Zip.Reduced

Installed builds of Unreal Engine 5.0, including the official 5.0.3 available at https://www.unrealengine.com/en-US/linux, need `Ionic.Zip.Reduced` in a few places but doesn't place it everywhere it's needed.
Copy it to
- `Engine/Binaries/DotNET/`
- `Engine/Binaries/DotNET/AutomationTools`
- `Engine/Binaries/DotNET/BuildTools`
either from one of  the places above, if any exist, or from a working copy of the git repository in which `Setup.sh` has been run.
```
$ cp $UE_ROOT/Engine/Binaries/DotNET/AutomationTool/Ionic.Zip.Reduced.dll $UE_ROOT/Engine/Binaries/DotNET/
```


# Broken Viewport Camera Controls Remote Desktop VNC

Not sure why it happens, but fixed by passing `-NoRelativeMouseMode` on the command line when starting Unreal Editor.
It's and SDL thing.
See
- https://forums.unrealengine.com/t/work-from-home-how-to-use-unreal-through-remote-desktop/141157
- https://wiki.libsdl.org/SDL_SetRelativeMouseMode


# Not Getting Indirect Lighting From Baked Lighting

This problem appeared with Unreal Engine 5.0.
Only seems to affect Linux/Vulkan, a scene that is broken works on Windows.
A forum discussion on the topic, including a Windows user:
https://forums.unrealengine.com/t/baked-global-illumination-in-ue5/514487
Though that may be a separate problem, I see a lot of Mac users in the thread.

I haven't yet found a solution to this problem.
Baked lighting does not produce any indirect light.

One [suggestion](https://discord.com/channels/187217643009212416/375022233875382274/1076284173951705149) is to enable the UDP Messaging plugin.


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


# Unable to find plugin 'Bridge'

While Unreal Engine 5 does include the Quixel Bridge plugin Epic Games chose to remove it from Linux builds, even the official 5.0.3 available at https://www.unrealengine.com/en-US/linux.
It is explicitly excluded here: https://github.com/EpicGames/UnrealEngine/blob/5.0.3-release/Engine/Build/InstalledEngineFilters.xml#L208
See Unreal Slackers Discord discussion: https://discord.com/channels/187217643009212416/375022233875382274/985976633376780358
Another about how to build it locally from a git working copy of the engine: https://discord.com/channels/187217643009212416/375022233875382274/1001741396048347157
Here we have two Unreal Engine installs, one with the official binaries in `/official` and one git working copy in `/git`.
The `git` one can be a clean working copy, we don't need to build it.
```bash
/official$ ./Engine/Build/BatchFiles/RunUAT.sh \
	BuildPlugin \
	-Plugin=/git/Engine/Plugins/Bridge/Bridge.uplugin \
	-Package=/tmp/Bridge  # Is it safe to send directly to ./Engine/Plugins/Bridge/?
/official$ mv /tmp/Bridge ./Engine/Plugins/Bridge/
```


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
