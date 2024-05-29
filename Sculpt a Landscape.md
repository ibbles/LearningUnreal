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


# References

- [_Advanced Skill Sets for Environment Art_ > _Landscape Sculpting and Landmass_ by Epic Online Learning 2022 UE4.27](https://dev.epicgames.com/community/learning/courses/Qwa/unreal-engine-advanced-skill-sets-for-environment-art/0Rz6/landscape-sculpting-and-landmass)
