A lightmap is a type of texture that contains [[Lighting]] information.
It is a type of acceleration structure that makes runtime lighting calculations faster.
The lightmap is generated, also called baked, ahead of time, for example while a project is being packaged.
[[Lighting]] making use of a Lightmap is called [[Static Lighting]] or baked lighting.
In Unreal Engine the tool that generates lightmaps is called [[Lightmass]].

Each [[Mesh]] that should receive [[Static Lighting]] need a lightmap UV channel.
This is a type of texture coordinate that guarantees that every point on the mesh map to a unique point on the lightmap.
There can be no overlaps.

When working with lightmaps it can help to switch the [[View Mode]] to Lighting Only.
And to disable [[Level Viewport]] > Show > Lighting Features > Screen Space Ambient Occlusion.
This makes it easier to see the contributions from [[Static Lighting]] only.

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


# References

- [_Introduction To Global Illumiation_, by Epic Games @ dev.epicgames.com, UE-4.20](https://dev.epicgames.com/community/learning/courses/yon/introducing-global-illumination/yo8/introduction-to-global-illumination)

