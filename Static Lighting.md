Static Lighting is also called Baked Lighting.
Lighting that is calculated at build-time.
More limited than [[Dynamic Lighting]] but also more performant at runtime.
Setting up Static Lighting is more complicated than [[Dynamic Lighting]], there are more settings to get right and more of a process to go through.
[[Dynamic Lighting]] is also typically easier to use during level design since we can see the end result immediately.

Static lighting can be disabled completely with [[World Settings]] > Lightmass > Advanced > Force No Precomputed Lighting.
Must build lighting once with Top Menu Bar > Build > Build Lighting Only to update everything.

A [[Light Sources]] produces Static Lighting if its [[Mobility]] ([[Light Mobility]]) is set to Static or Stationary.
Setting the [[Mobility]] to Movable turns the light into a dynamic light.

The scene lighting needs to be rebuilt every time a static or stationary light or object is changed in the scene.
If there are at least one static light in the scene and the scene is modified within that lights influence volume then a message saying "LIGHTING NEEDS TO BE REBUILT" is typically printed to the  top-right corner of the viewport.


## References

- [_# Lighting in Unreal Engine 5 for Beginners_ by William Faucher @ youtube.com](https://youtu.be/fSbBsXbjxPo?t=309)

