By default Unreal Editor will disable the [compositor](https://unix.stackexchange.com/questions/359257/what-is-a-compositor-in-general-and-which-gives-the-best-performance-ubuntu) when started.
This is good for graphics performance but reduces usability of the machine at large.
If you don't need maximum graphics performance and want the convenience of window compositing then the disabling can be disabled.
There isn't a setting built into Unreal Engine for this, but we can use [Window Manager](https://wiki.archlinux.org/title/window_manager) settings or SDL hints.
If you have building Unreal Engine from source then it's possible to set the hint from there as well.

# Window Manager Settings

- KDE: Disable System Settings > Display And Monitor > Compositor > Allow Applications To Block Compositing.
- [[TODO: List other window managers as well.]]


# SDL Hint

The [`SDL_HINT_VIDEO_X11_NET_WM_BYPASS_COMPOSITOR`](https://metacpan.org/pod/SDL2::hints#SDL_HINT_VIDEO_X11_NET_WM_BYPASS_COMPOSITOR) hint controls whether or not compositing should be disabled when an application is started.
If compositing has already been disabled then setting this won't make any difference.
It can be set either as an environment variable or by the application.


## Environment variable

Set the `SDL_VIDEO_X11_NET_WM_BYPASS_COMPOSITOR` environment variable to `0` before launching the Unreal Editor.
Using Fish:
```
➤ env SDL_VIDEO_X11_NET_WM_BYPASS_COMPOSITOR=0 UnrealEditor
```
using Bash:
```
$ export SDL_VIDEO_X11_NET_WM_BYPASS_COMPOSITOR=0 UnrealEditor
```


## Unreal Engine source

I'm in no way certain that this is the proper way to do this.
It would be nice to have a setting instead of always let the compositor be, but I don't know how to do that.

In `Runtime/ApplicationCore/Private/Linux/LinuxPlatformApplicationMisc.cpp` there is a function named `FLinuxPlatformApplicationMisc::InitSDL`.
Add the following code snipped to that function, within the `if (!GInitializedSDL)` block.
There are a few other `SDL_SetHint` calls, place it next to those.
The `WITH_EDITOR` bit will ensure that we only disable compositing for the editor, a packaged game will still disable it.

```cpp
#if WITH_EDITOR
	SDL_SetHint(SDL_HINT_VIDEO_X11_NET_WM_BYPASS_COMPOSITOR, "0");
#endif
```
