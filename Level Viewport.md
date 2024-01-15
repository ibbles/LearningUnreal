The Level Viewport is the part of the [[Level Editor]] where we can see the contents of our level.

There are various [[Viewport Camera Controls]].

The [[Transform Gizmo]] is used to move objects in the level.

There are several debug and performance [[View Mode|View Modes]] that can be selected from the top-left of the viewport.
The default is Lit.
A number of things to render can be toggled from the Show menu in the top-left of the Level Viewport.
Various statistics can be show on the Level Viewport with the [[Stat]] command.

Press and hold Ctrl and type L, keep holding Ctrl, and move the mouse to rotate the Directional Light in the level.
Release Ctrl to accept the new light setup.

Type G to toggle Game Mode, which when enabled hides icons for non-visible things such as [[Light Source|Light Sources]].

Type F11 to maximize the viewport, type it again to unmaximize.

With an [[Actor]] selected, type End to place it on whatever is beneath it.
Collision shapes will be used if available, otherwise the origin of the object is placed on top of the thing below.

Left-click an object in the Viewport to select it.
If this doesn't work then check the Allow Translucent Selection option in [[Main Tool Bar]] > Gear.
The selected object is highlighted in the [[Outliner]] panel and the [[Property|Properties]] for that object is shown in the [[Details Panel]].
Hold Shift when clicking to select multiple objects.
Hold Ctrl when clicking to toggle selection of the object.
(
Check if I got Shift/Ctrl correct.
)
Hold Ctrl+Alt and `LMB` drag to select all objects in a box.
Right-click an object in the Viewport to see actions for that object.


# Hamburger Menu

The hamburger menu, three lines in the top-left, contains settings for the viewport.

- Realtime
- Show FPS
- Show Stats
- Projection settings such as field of view.
- Layouts
	- See _Layouts_ below.


# Layouts

The Level Viewport can show the level from multiple views at the same time.
Up to four.
These are called panes.
Select the number of panes with Level Viewport > top-left > hamburger menu > Layouts.
Each panel can be configured to show perspective, orthographic, top, right, back, etc, wireframe, unlit, lit, cinematic, and so on independently.
One pane can be maximized.
A grid button in the top-right of the Level Viewport unmaximizes the pane so that all panes become visible.
When unmaximized the unmaximize button becomes a maximize button.
Any pane can be maximized.


# Multiple Viewport

We can have multiple viewports  open at once.
Enabled from Top Menu Bar > Viewports > Viewport \[1-4\].


# References

- [_Unreal Engine Editor Fundamentals > Viewport Widow_ by Epic Games @ dev.epicgames.com 2023](https://dev.epicgames.com/community/learning/courses/D95/unreal-engine-editor-fundamentals/XekP/unreal-engine-viewport-window)

