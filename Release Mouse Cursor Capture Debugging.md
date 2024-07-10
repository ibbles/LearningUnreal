In some circumstances Unreal Engine captures the mouse cursor to be within the viewport.
If the engine crashes at that point then the mouse cursor may remain bound/locked to that area of the screen.

The mouse cursor is also locked while we are inside a click handler, for example button callback code or `PostEditChangeProperty`.
We sometimes want to debug code triggered by a mouse click.
This is problematic because the mouse is captured by Unreal Editor while the click is being processed.
This makes it impossible to use the mouse in other applications, mouse clicks are ignored.

Sometimes Unreal Editor bugs out when ending a Play In Editor session and leaves the mouse cursor captured.

This note lists some ways to try and prevent or mitigate these issues.


# No Grab No Capture Project Setting

In the [[Project Settings]] there is Engine > Input > Viewport Properties > {Capture Mouse On Launch, Default Viewport Mosue Ca..., Default Viewport Mouse Lo...}.
Set these to false, No Capture, and Do Not Lock.

Not sure what unwanted effects this will have, so I usually don't do this.


# KDE Setting

In KDE there is System Settings > Keyboard > Advanced > Miscellaneous Compatibility Options > Allow Breaking Grabs With Keyboard Actions.
Not sure that that does, but was [recommended](https://discord.com/channels/187217643009212416/375022233875382274/1079942897874505849) to disable it if the other solutions doesn't work.


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


# LLDB Breakpoint Attach Script


See https://forums.unrealengine.com/t/debugging-in-rider-on-ubuntu-linux-gnome-causes-editor-or-game-to-freeze-on-breakpoints-causing-no-mouse-to-appear/835140/6

Add the following to `~/.lldbinit`, or wherever your LLDB init script is stored:
```shell
target stop-hook add --one-liner "p ::UngrabAllInputImpl()"
```

If this causes issues when debugging non-Unreal applications, see the linked post for a slightly more complicated but Unreal-specific solution.


# References

- [_Debugging in Rider on Ubuntu Linux (gnome) causes editor or game to freeze on breakpoints, causing no mouse to appear_ by noisycat @ forums.unrealengine.com 2024 UE5.3](https://forums.unrealengine.com/t/debugging-in-rider-on-ubuntu-linux-gnome-causes-editor-or-game-to-freeze-on-breakpoints-causing-no-mouse-to-appear/835140)

