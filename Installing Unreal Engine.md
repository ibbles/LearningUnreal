Download `Linux_Unreal_Engine_<VERSION_3>.zip` and `Linux_(Fab|Bridge)_<VERSION_2>.0_<DATE>.zip` from [https://www.unrealengine.com/en-US/linux](https://www.unrealengine.com/en-US/linux).
Create a directory for the installation and unzip both Zip archives there, run Unreal Editor to let it initialize itself.
```shell
/media/s1300/unreal_engine
➤ mkdir <VERSION_3>
➤ unzip -q ~/Downloads/Linux_Unreal_Engine_5.4.2.zip -d ./<VERSION_3>/
➤ unzip -q ~/Downloads/Linux_Bridge_5.4.0_2024.0.1.zip -d ./<VERSION_3>/
➤ ./<VERSION_3>/Engine/Binaries/Linux/UnrealEditor
```

This should add the new engine version to
- `~/.config/Epic/UnrealEngine/Install.ini`
- `~/.config/Epic/UnrealEngineLauncher/LauncherInstalled.dat`

(
Not sure how the above files relate to each other.
I think `Install.ini` is older and `LauncherInstalled.dat` replaces it.
)

If not, and  if opening projects with a `.uproject` file with an Engine Association pointing to that engine version fails, you may need to add it manually.
My `Install.ini` contains rows with the following pattern:
```
[Installations]
UE_<VERSION_2>=/media/s1300/unreal_engine/<VERSION_3>
<VERSION_2>=/media/s1300/unreal_engine/<VERSION_3>
<VERSION_3>=/media/s1300/unreal_engine/<VERSION_3>
```

For example:
```[Installations]
UE_5.4=/media/s1300/unreal_engine/5.4.2
UE_5.3=/media/s1300/unreal_engine/5.3.2
5.4=/media/s1300/unreal_engine/5.4.2
5.4.2=/media/s1300/unreal_engine/5.4.2
5.3=/media/s1300/unreal_engine/5.3.2
5.3.2=/media/s1300/unreal_engine/5.3.2
```

Example `LauncherInstalled.dat` :
```json
{
    "InstallationList": [
        {
            "AppName": "UE_5.4",
            "InstallLocation": "/media/s1300/unreal_engine/5.4.2"
        },
        {
            "AppName": "UE_5.3",
            "InstallLocation": "/media/s1300/unreal_engine/5.3.2"
        }
    ]
}
```

# LLDB Debugger Configuration

See also [[Debug C++ Code]].

## Reduce Launch Time With Delayed Symbol Loading

For quicker launches when starting with the LLDB debugger consider adding the following to your `~/.lldbinit`:
```
settings set symbols.load-on-demand true
settings set target.preload-symbols false
settings set plugin.jit-loader.gdb.enable off
```

The drawback is that there will be a delay on the first breakpoint in a module since the debug symbols will be loaded then instead.


## Ungrab Mouse On Breakpoint

Add the following to your `~/.lldbinit`:
```
settings set target.inline-breakpoint-strategy always
target stop-hook add --one-liner "p ::UngrabAllInputImpl()"
```
