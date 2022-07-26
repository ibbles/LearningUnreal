Foliage Mode is an [[Editor Mode]] where we can place foliage.
We say that we paint foliage onto things.
Foliage is kinda like [[Instanced Static Mesh]], but with more features.

Note that there are two settings for density.
One is in the per-foliage type Details panel at the bottom of the Foliage panel.
One is the global Paint Density in the Paint > Brush Options of the Foliage panel.

There is a filter to control which type of things we can paint foliage on.
- Landscape
- Static Mesh
- Foliage
- BSP

The paint cursor may become misaligned with the mouse cursor when piloting a [[Camera]] with a custom aspect ratio, such as a [[Cine Camera]].


# Foliage Painting

To paint foliage first create a [[Static Mesh Foliage]] with [[Content Browser]] > right-click > Foliage > Static Mesh Foliage.
Static Mesh Foliage contains a Mesh property that should be set to the [[Static Mesh Asset]] that should be instanced.
The [[Static Mesh Asset]] should have [[Static Mesh Editor]] > Details panel > LOD Settings > LOD Group property set to Foliage if you are using static lighting, otherwise the foliage will be black.


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