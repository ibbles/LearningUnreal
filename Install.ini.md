Unreal Engine store a list of installed engine versions in `~/.config/Epic/UnrealEngine/Install.ini`, in the `[Installations]` section.
The file has the following structure:
```
[Installations]
<HASH>=<PATH>
```

I like to edit this file to add entries with more descriptive names, instead of hash values.
Those names can be used as `EngineAssociation` in `.uproject` files.
Such entries should be placed after the `<HASH>` entries, because of how `FDesktopPlatformLinux::EnumerateEngineInstallations` processes the list.
It does reverse look-ups from the engine path to find the hash value and it will only find the first entry for each path.
So the first key for an engine path must be a valid hash string, otherwise it becomes the all-zero default hash for all engine installations, causing `EnumerateEngineInstallations` to believe there is only one.
Example file:
```
[Installations]
B4BA80E5-061F-406C-B07C-A6C2AC42AE61=/media/s1300/unreal_engine/5.3.0
9003871E-1DF8-4B81-ABE7-60CEBB146BCA=/media/s1300/unreal_engine/5.2.1
4CE5D4F1-5FB0-49BD-BF9D-BD3085C8C7D8=/media/s1300/unreal_engine/5.1.1
ACD168DC-A353-4632-936F-026F02096F1C=/media/s1300/unreal_engine/5.0.3

5.3.0=/media/s1300/unreal_engine/5.3.0
5.2=/media/s1300/unreal_engine/5.2.1
5.2.1=/media/s1300/unreal_engine/5.2.1
5.1=/media/s1300/unreal_engine/5.1.1
5.1.1=/media/s1300/unreal_engine/5.1.1
5.0=/media/s1300/unreal_engine/5.0.3
5.0.3=/media/s1300/unreal_engine/5.0.3
```

I list both two-digit and three digit version numbers, and update the two-digit version whenever I install a new path release.
Then in the `.uproject` file I can write `5.0` if I want whatever is the latest `5.0` version, and `5.0.3` if I want that exact version.

The `<HASH>` value for an Unreal Engine installation can be found in `$UE_ROOT/Engine/Build/InstalledBuild.txt`.

This entire setup may be bad idea, as explained on Discord and described in the following section.


# Some Input From Discord

https://discord.com/channels/187217643009212416/375022233875382274/1052281858387361792

For future reference - `Install.ini` located in `~/.config/Epic/UnrealEngine/` can only store engine locations under GUIDs, anything else is converted to GUID before its used. The file I needed to edit is `~/.config/Epic/UnrealEngineLauncher/LauncherInstalled.dat`. I figured out the minimal structure needed and my current state is as follows: My `Install.ini`:

```
[Installations]
```

My `LauncherInstalled.dat`:

```
{
    "InstallationList": [
        {
            "AppName": "UE_5.1",
            "InstallLocation": "~/.var/UnrealEngine"
        }
    ]
}
```

Everything works great, my project has `"EngineAssociation": "5.1",`, opens without needing to select engine installation and opening it doesn't change the `EngineAssociation`.

A later discussion: [Unreal Source Discord thread 2024](https://discord.com/channels/187217643009212416/375022233875382274/1199994122124148837)

