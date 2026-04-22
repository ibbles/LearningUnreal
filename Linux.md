See also [[Window Manager Compositing On Linux]].

# Wayland

You may need to set Legacy X11 App Support > Allow legacy X11 apps to read keystrokes typed in all apps to Always in the system settings [(1)](https://unrealcommunity.wiki/linux-quickstart-faqs-8f7980).

[UE5.3 cannot input text find function on Linux Wayland @ forums.unrealengine.com 2024](https://forums.unrealengine.com/t/ue5-3-cannot-input-text-find-function-on-linux-wayland/1799561)

For Unreal Engine 5.7 and thereabout, also start Unreal Editor with the X11 video driver [(1)](https://unrealcommunity.wiki/linux-quickstart-faqs-8f7980):
```shell
env -u WAYLAND_DISPLAY SDL_VIDEODRIVER=x11 ~/UE5/Engine/Binaries/Linux/UnrealEditor /home/USER/PROJECTFOLDER/GAME.uproject
```

I'm not sure if the `-u WAYLAND_DISPLAY` bit, which removes the `WAYLAND_DISPLAY` environment variable, is necessary or not.
Not mentioned in the Linux QuickStart [(1)](https://unrealcommunity.wiki/linux-quickstart-faqs-8f7980) but I've seen it mentioned on Unreal Source [(2)](https://discord.com/channels/187217643009212416/375022233875382274/1496417776242393189)


If you are having issues with tooltips and/or double-clicking then disable tooltips with `Slate.EnableTooltips=False` in `DefaultEngine.ini` [(1)](https://unrealcommunity.wiki/linux-quickstart-faqs-8f7980).

# References

- 1: [_Linux QuickStart / FAQs_ @ unrealcommunity.wiki](https://unrealcommunity.wiki/linux-quickstart-faqs-8f7980)
- 2: [_Unreal Source_ message by David Boura @ discord.com 2026](https://discord.com/channels/187217643009212416/375022233875382274/1496417776242393189)

