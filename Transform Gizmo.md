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
Drag the plane markers to translate, rotate, or scale the object along that plane.

Switch between the transform modes with the corresponding buttons in the top-right of the Level Viewport or the keyboard shortcuts `Q`, `W`, `E`, and `R`.
Or press spacebar to cycle between them.

Next to the translate, rotate, scale buttons in the top-right of the Level Viewport is the global / local space toggle.
This is an all-or-nothing switch, you are either in the world space or the object's local space.
There is no way to operate in the coordinate frame of a parent [[Component]].

There is a snapping option for all three transform modes.
Is selected in the top-right of the [[Level Viewport]].
Not free snapping, only a list of pre-defined values.
Snapping can be toggled on a per-mode basis.
The snapping sizes can be set at [[Editor Preferences]] > [[Level Editor]] > Viewports >  Grid Snapping.

# Grid

When translation snapping is enabled we can imagine a grid placed over the world and any object translation we do is snapped to one of those grid boundaries.
The object is either moved a full grid cell or not at all.

Additional snapping options are available from [[Actor]] > right-click > Transform > Snap/Align.
Some of the options have keyboard shortcuts.

Keyboard controls:
- `]`: Increase grid size.
- `[`: Decrease grid size.
- `Shift` + `]`: Increase angle snap.
- `Shift` + `[`: Decrease angle snap.
- `Ctrl` + `End`: Snap origin to grid.
  - I think this moves the selected object to the nearest grid point. Not sure though.
- `Alt` + `End`: Snap pivot to floor.
  - Move the selected object down until its pivot point hit something.
- `End`: Snap to floor.
  - Move the selected object down until it collides with something.

# References

- [_Unreal Engine Editor Fundamentals > Viewport Window_ by Epic Games @ dev.epicgames.com 2023](https://dev.epicgames.com/community/learning/courses/D95/unreal-engine-editor-fundamentals/XekP/unreal-engine-viewport-window)
- [_Becoming an Environment Artist in Unreal Engine_ > _Using the Grid and Snapping Tools_ by Epic Games @ dev.epicgames.com/courses 2020 UE4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/dXw/unreal-engine-using-the-grid-and-snapping-tools)


