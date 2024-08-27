See also [[Creating a Landscape]]

Sculpting is the process of raising or lowering the surface of a [[Landscape]] around a particular coordinate.
An alternative to sculpting is [[Landmass]].

If Edit Layers is disabled then sculpting will modify the [[Landscape]]'s [[Height Map]] directly.

# Edit Layers

If Edit Layers are enabled then the Landscape panel of the [[Landscape Mode]] will contain a Edit Layers category.
The Edit Layers category contains one or more layers.
Each layer acts as a [[Height Map]] and the [[Landscape]]'s final [[Height Map]] is a combination of these layers.
When using the sculpt tools with Edit Layers enabled your sculpting will be made to one of these layers.
To create a new layer right-click an existing layer and select Create.
You can enable or disable individual layers using the eye icon on each layer.
This makes it possible to see what each layer contributes to the final [[Height Map]], and to help identify which layer should be edited to achieve a particular change.

# Visibility

See [[Invisible Landscape Patches]].

# Ramp

The Ramp is a sculpt tool, available from the main toolbar in the [[Landscape Mode]] Sculpt tool bar.
It creates flat, inclined surfaces, i.e. ramps.
A ramp is created from a begin control point and an end control point.
Create the control points by clicking on the [[Landscape]] in the [[Level Viewport]].
The first click creates the begin control point, the second click creates the end control point.
Clicking on the [[Landscape]] again will move the selected control point to the clicked location.
Change the selected control point by Ctrl + click on the other control point.
Clicking one the [[Landscape]] will now move that control point instead.
You can also move the control point using the [[Transform Gizmo]].
Click the Add Ramp button to finalize the ramp, i.e. modifying either the selected edit layer or the [[Height Map]].

# Copy

The Copy tool is used to copy [[Landscape]] heights from one location to another, possibly transformed.
The Copy tool works kind of like the clone tool in an image paint program.
But instead of having a read brush location and a write brush location the Copy tool reads from a data store held by a gizmo in an internal storage.
The gizmo defines a volume in space.
Place, rotate and scale the gizmo volume to cover the region of the [[Landscape]] you want to copy from, then click the Copy Data To Gizmo button to copy the heights to the gizmo's internal storage.
Move the gizmo to where you want to paste.
You can also rotate and scale the gizmo to rotate and scale the copied heights.
Can only rotate in the XY plane, cannot tilt it.
Use one of the brushes to apply the copied heights from the gizmo's internal storage to the [[Landscape]].
Select a Brush type at Landscape panel > Copy > Landscape Editor > Brush Type.
With the Simple Circular Brush you can paint where you  want to apply new heights within the gizmo's volume.
Use a low Tool Strength and/or Brush Falloff to get smooth transitions.
With the Gizmo Brush the entire copied height data is applied all at once with a single click.

The gizmo's internal height data is rendered with yellow lines,
which makes it very difficult to see the results while painting.
I switch back and forth between the Copy tool and the Select tool to turn the yellow lines on and off.

Sometimes the gizmo becomes unselected.
Re-select it by switching to the Select tool and then back to the Copy tool.

The Copy tool takes the current selection, made with the Select tool, into account.
With the Select tool active, paint the parts of the [[Landscape]] you want to copy.
Switch to the Copy tool.
Click the Fit Gizmo To Selected Regions button.
Click the Copy Data To Gizmo button.
Only the selected parts within the gizmo's volume will be copied.
Continue as described above.
When painting the heights to a new location only copied heights will be applied,
areas that does not correspond to a copied height is left unchanged.

See [_Landscape Copy Tool_ by Epic Games @ dev.epicgames.com/documentation](https://dev.epicgames.com/documentation/en-us/unreal-engine/landscape-copy-tool-in-unreal-engine).


# References

- [_Advanced Skill Sets for Environment Art_ > _Landscape Sculpting and Landmass_ by Epic Online Learning 2022 UE4.27](https://dev.epicgames.com/community/learning/courses/Qwa/unreal-engine-advanced-skill-sets-for-environment-art/0Rz6/landscape-sculpting-and-landmass)
- [_Landscape Copy Tool_ by Epic Games @ dev.epicgames.com/documentation](https://dev.epicgames.com/documentation/en-us/unreal-engine/landscape-copy-tool-in-unreal-engine)
