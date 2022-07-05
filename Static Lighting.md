Static Lighting is also called Baked Lighting.
Lighting that is calculated at build-time.
More limited than [[Dynamic Lighting]] but also more performant at runtime.
Setting up Static Lighting is more complicated than [[Dynamic Lighting]], there are more settings to get right.
[[Dynamic Lighting]] is also typically easier to use during level design since we can see the end result immediately.

A [[Light Sources]] produces Static Lighting if its [[Mobility]] is set to Static or Stationary.
Setting the [[Mobility]] to Movable turns the light into a dynamic light.

If there are at least one static light in the scene and the scene is modified within that lights influence volume then a message saying "LIGHTING NEEDS TO BE REBUILT" is typically printed to the  top-right corner of the viewport.
