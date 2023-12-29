The Transform Gizmo is the translate / rotate / scale tool in the [[Level Viewport]].

With an object ([[Actor]] or [[Component]]) selected in the Viewport we can change its transformation with the transform gizmo.
The transform gizmo has three modes:
- None (select) (`Q`)
- Translate (`W`)
	- Shown as three arrows, one for each axis.
- Rotate (`E`)
	- Shown as three circular bands, one for each axis.
- Scale (`R`)
	- Shown as three lines with boxes at the end, one for each axis.

Drag the arrows, bands, or boxes to translate, rotate, or scale the object, respectively.
The selected axis turn yellow.
Drag the plane markers to translate or scale the object along that plane.

Switch between the transform modes with the corresponding buttons in the top-right of the Level Viewport or the keyboard shortcuts.
Or press spacebar to cycle between them.

Next to the translate, rotate, scale buttons in the top-right of the Level Viewport is the global / local space toggle.
This is an all-or-nothing switch, you are either in the world space or the object's local space.
There is no way to operate in the coordinate frame of a parent [[Component]].

There is a snapping option for all three transform modes.
Is selected in the top-right of the Level Viewport.
Not free snapping, only a list of pre-defined values.
Snapping can be toggled on a per-mode basis.
The snapping sizes can be set at [[Editor Preferences]] > [[Level Editor]] > Viewports >  Grid Snapping.


# References

- [_Unreal Engine Editor Fundamentals > Viewport Window_ by Epic Games @ dev.epicgames.com 2023](https://dev.epicgames.com/community/learning/courses/D95/unreal-engine-editor-fundamentals/XekP/unreal-engine-viewport-window)

