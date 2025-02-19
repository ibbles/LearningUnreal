
See also [[Input Mode]].

# Mouse Buttons

(
This text describe the old mappings-based input system. Unreal Engine 5 introduced a new system called [[Enhanced Input]].
)

Mouse button events are listened to in the same way as all other [[Input Event]].
Each mouse button press and release generate an [[Action Event]].
Set up an [[Action Event]] for a mouse button at [[Project Settings]] > Engine > Input > [[Input Bindings]] > [[Action Mappings|Action Mapping]].
You can use an Axis Mapping if you want, but I think Action Mapping makes more sense since a click is an instantaneous discrete event.
Click the `+` button next to Action Mappings.
Enter a name for the Action Mapping, a short description of what should happen when the action is performed.
By default there is no key binding for the newly created action, shown as None beneath the name.
Either select one of the mouse buttons in the drop-down under the Mouse group, or click the keys icon to the left and then click the mouse button.
Now we have a mapping from the mouse button to the new action.
You can act on the event either in a [[Blueprint Event]] listener or using a [[Action Event In C++]].


# Cursor Location In World Space
In a [[Player Controller]] we can find the current location, in world space, of the mouse cursor using `DeprojectMousePositionToWorld`.
It takes two output parameters:
- `FVector WorldLocation`
- `FVector WorldDirection

The World Location is a location that is hit when tracing a ray from the camera through the mouse cursor location on the near clip plan and into the world.
I don't know the engine does an actual intersection test against geometry in the world, or if we get a default distance from the camera.

The World Direction is the direction of the ray.

```cpp
FVector WorldLocation;
FVector WorldDirection;
DeprojectMousePositionToWorld(
	WorldLocation, WorldDirection);
```


# Overlap / Cursor Over Event

A [[Static Mesh]] can trigger On Begin Cursor Over and On End Cursor Over [[Event]]s.
For this to work the [[Player Controller]] must have Mouse Over Events enabled.
This can be enabled from a [[Blueprint Visual Script]] with the Set Enable Mouse Over Events node.

In the [[Blueprint Class]] that has the [[Static Mesh Component]] select the Component in the Components panel.
In the [[Details Panel]], in the Events category Click the `+` button next to On Begin Cursor Over and / or On End Cursor Over.
[[Event Node]]s are added to the [[Event Graph]].

I tried to implement this myself using [[Enhanced Input]] and Mouse 2D XY-Axis,
since I wanted to be able to enable / disable it using [[Mapping Context]],
but the Mouse 2D XY-Axis event would only trigger while a mouse button was held,
which is not what I want.

# Event Actor On Clicked

See [[Event Actor On Clicked]].


# Ungrab Mouse Cursor During Debugging

If a breakpoint is hit from a click callback, such as when clicking a button, then the mouse cursor will be grabbed by the click event.
It must be ungrabbed before it can be used to click in the debugger or other windows.

## Ungrab Mouse Manually

Run the following commands to ungrab the mouse.
```bash
setxkbmap -option grab:break_actions
xdotool key XF86Ungrab
```

I have them in a script bound to a global keyboard shortcut.


## Ungrab Mouse On Breakpoint

Add the following to your `~/.lldbinit`:
```
settings set target.inline-breakpoint-strategy always
target stop-hook add --one-liner "p ::UngrabAllInputImpl()"
```


# References
- [_C++ For Unreal Engine (Part 1) | Learn C++ For Unreal Engine | C++ Tutorial For Unreal Engine_ 5:30:02 DeprojectMousePositionToWorld by Nerd's Academy. 2022](https://youtu.be/47z5sVbxmUo?list=PLkDceauvDXDy23KPR7tU2lkA9C69MzI0k&t=19802)
- [_C++ For Unreal Engine (Part 1) | Learn C++ For Unreal Engine | C++ Tutorial For Unreal Engine_ 5:44:22 Mouse Button Action Mapping by Nerd's Academy. 2022](https://youtu.be/47z5sVbxmUo?list=PLkDceauvDXDy23KPR7tU2lkA9C69MzI0k&t=20672)

