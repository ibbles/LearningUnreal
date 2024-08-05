Foliage Mode is an [[Editor Mode]] where we can place foliage.
We say that we paint foliage onto things.
Foliage is kinda like [[Instanced Static Mesh]], but with more features.

Note that there are two settings for density.
One is in the per-foliage type Details panel at the bottom of the Foliage panel.
One is the global Paint Density in the Paint > Brush Options of the Foliage panel.

There is a filter to control which type of things we can paint foliage on.
Check the boxes next to the things to paint on.
- Landscape
- Static Mesh
- Foliage
- BSP


The paint cursor may become misaligned with the mouse cursor when piloting a [[Camera]] with a custom aspect ratio, such as a [[Cine Camera]].


# Foliage Painting

To paint foliage first create a [[Static Mesh Foliage]] with [[Content Browser]] > right-click > Foliage > Static Mesh Foliage.
[[Static Mesh Foliage]] contains a Mesh property that should be set to the [[Static Mesh Asset]] that should be instanced.
The [[Static Mesh Asset]] should have [[Static Mesh Editor]] > Details panel > LOD Settings > LOD Group property set to Foliage if you are using static lighting, otherwise the foliage will be black.

[[Static Mesh Foliage]] types can be added to the foliage list in the Paint tab of the Foliage panel in the [[Foliage Mode]].
Below the foliage list you can set various parameters for the currently selected foliage.
Examples of parameters are density, scale, random rotation, and Z Offset.
Some of these settings are aliases for the [[Static Mesh Foliage]] [[Asset]],
while some are foliage painting tool specific and cannot be set in the [[Asset]].
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


# No Instances Are Created

Things to check if no instances are being created, listed from top to bottom in the Foliage panel:
- Is Brush Options > Brush Size set large enough?
	- You should see the brush as a translucent dome in the [[Level Viewport]] when painting.
- Is Brush Options > Paint Density set high enough?
	- TODO Move text from below.
- Is any [[Static Mesh Foliage]] activated in the Foliage panel?
	- If none is active than nothing will be painted.
	- Selecting and activating is not the same thing.
	- A [[Static Mesh Foliage]] is active if it is not grayed out and has a check mark when hovered.
- Is Painting > Density high enough?
	- If the density is too low and there already are some foliage instances where you paint then no new will be created.
	- A density of 10 for "medium sized" objects is reasonable.
	- Test if this is the problem by painting in an empty area, i.e. an area where the density is zero.
- Is Painting > Radius small enough?
	- If the radius is high then no new instance will be created nearby already existing instances.
	- A radius of 50 is reasonable for "medium sized" objects.
	- Test if this is the problem by painting in an empty area.
- Is Placement > Ground Slope Angle incorrect?
	- When painting the tool checks the slope at the painted location and rejects new instances if it is outside of the given angle range.
	- 0 means flat / horizontal, 90 means a wall.
	- I don't know how the range works when Max is > 180.
		- It seems the range then goes backwards, so the range `[0, 359]` is a one-degree range tilting backwards, whatever that means.
