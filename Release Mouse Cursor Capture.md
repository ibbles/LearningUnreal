In some circumstances Unreal Engine captures the mouse cursor to be within the viewport.
If the engine crashes at that point then the mouse cursor may remain bound/locked to that area of the screen.

The mouse cursor is also locked while we are inside a click handler, for example button callback code or `PostEditChangeProperty`.
We sometimes want to debug code triggered by a mouse click.
This is problematic because the mouse is captured by Unreal Editor while the click is being processed.
This makes it impossible to use the mouse in other applications, mouse clicks are ignored.


# Ungrab Uncapture Release

In some Window Managers it is possible to uncapture the mouse cursor again.
In KDE, try Alt+Space or Alt+F2, which should open KRunner and give it focus.
In Gnome, try Super+Tab.
In Gnome, it may also help to enable Gnome Tweaks > Windows > Window Focus > Focus on Hover.

You can also create a script that uses `xdotool` to move `windowfocus` to you IDE and bind that to a global hotkey.
Another alternative is a script with `xdotool key XF86Ungrab` and bind that to a global hotkey.
May need to run `setxkbmap -option grab:break_actions` before any of the above commands.

I have the following script bound to Super+U, for Ungrab.
```bash
#!/bin/bash

# This script tries to abort a mouse cursor grab action. For example, when
# debugging GUI applications with a debugger attached in a click callback we may
# end up in a state where the current halted click prevent any new clicks from
# registering making it difficult to use IDE-integrated debuggers.

setxkbmap -option grab:break_actions
xdotool key XF86Ungrab
```
