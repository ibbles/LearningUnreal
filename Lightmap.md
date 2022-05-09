# Volumetric Lightmap
The Volumetric Lightmap can be visualized with Viewport > Show > Visualize > Volumetric Lightmap.
Will show up as spheres arranged in a regular grid in the world where there is a Lightmass Importance Volume.
(
I did not get this to show anything.
What is needed for the spheres to show up?
I have a [[Lightmass Importance Volume]] and a [[Point Light]] with [[Mobility]] set to Static.
I had to [[Baked Lighting|Bake Lighting]].
But the spheres are very very bright. As in the entire screen is while bright.
)

The density/resolution of the Volumetric Lightmap is controlled with Volumetric Lightmap Detail Cell Size.
Don't know where this setting is, not in the [[Project Settings]] and not on a [[Directional Light]], and not on a [[Lightmass Importance Volume]].

The Lightmap density can be visualized with Viewport > Lit > Optimization Viewmodes > Lightmap Density.
Expensive areas will show up in red.
A grid pattern show the size of the Light Map texture on each mesh.
Not sure how to interpret the size of the squares, but smaller means more expensive.
Not sure what a reasonable size would be, of when one would want smaller or larger squares.
Find the red meshes and override the scale/density.
Not sure what this means exactly.
[[Static Mesh Editor]] > Details panel > LOD 0 > Build Settings > Min Lightmap Resolution?
[[Static Mesh Editor]] > Details panel > General Settings > Light Map Resolution?
[[Static Mesh Component]] > Details panel > Lighting > Overriden Light Map Resolution?
Something else?
