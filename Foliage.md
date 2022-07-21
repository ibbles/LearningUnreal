Foliage is a way to add many instances of a collection of [[Static Mesh|Static Meshes]] easily.
Adding Foliage is done with the [[Foliage Mode]].
In this mode we can paint foliage into our level.

To add foliage first create a **Static Mesh Foliage** with [[Content Browser]] > right-click > Foliage > Static Mesh Foliage.
Static Mesh Foliage contains a Mesh property that should be set to the [[Static Mesh Asset]] that should be instanced.
The [[Static Mesh Asset]] should have [[Static Mesh Editor]] > Details panel > LOD Settings > LOD Group property set to Foliage if you are using static lighting, otherwise the foliage will turn black.

Static Mesh Foliage types can be added to the foliage list in the Paint tab of the Foliage panel in the [[Foliage Mode]].
Below the foliage list you can set various parameters for the currently selected foliage.
Examples of parameters are density, scale, random rotation, and Z Offset.
(
Test if editing the Static Mesh Foliage type modifies already placed instances.
Document the result here.
)

Add foliage instances to the level by activating one or more foliage types by enabling the checkbox in the foliage list and then click-and-drag in the Viewport.
Remove foliage instances by holding Shift while painting.

Note the difference between "selecting" and "activating" a foliage type.
Selecting means that its properties are shown below the foliage list.
Activating means that instances of the foliage type is created when painting.

The density settings for each foliage type can be scaled while painting by changing Brush Options > Paint Density.

To make a realistic looking scene it if often necessary to add [[Foliage Wind]].


# Warnings

## The Total Lightmap Size Of This InstancedStaticMeshComponent Is Large

This can appear in the Message Log panel when [[Building Lighting]] for a level with foliage.
The [[Lightmap]] size for foliage is reduced in [[Foliage Mode]] > Foliage panel > select one or more [[Foliage Type]]s > Details > Instance Settings > Light Map Resolution.
By enabling the override checkbox and reducing the value we reduce the [[Lightmap]] size for our foliage instances.
This should fix the warning, possibly improve performance, and reduce memory usage.

[_Use Fix and optimize foliage in unreal_ - Light Map Resolution, by Batnobie X @ youtube.com. 2021](https://youtu.be/jcZ5V8qFwgE?t=100)
