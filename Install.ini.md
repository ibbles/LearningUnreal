
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

Everything works great, my project has `"EngineAssociation": "5.1",`, opens without needing to select engine installation and opening it doesn't change the EngineAssociation.


