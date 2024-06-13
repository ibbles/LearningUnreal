This note describe the [[View Mode|View Modes]] that deal with [[Lighting]].


# Lighting Only

Renders the scene with lights only, no textures.
This is useful when working with [[Static Lighting]] and [[Lightmap|Lightmaps]] since it makes the contributions from those more apparent.


# Lightmap Density

Found at [[View Mode]] > Optimization Viewmodes > Lightmap Density.

This [[View Mode]] will color each object that has a Lightmap based on the world-space size of the Lightmap texels.
It will also render a grid pattern to help with comparing Lightmap density on different objects.
You want everything to be about the same color, with more green (I think) and a finer/smaller grid pattern on objects you want to receive more high-quality shadows.
Blue means  that the [[Lightmap]] resolution is too low.
Green means that the [[Lightmap]] resolution is good.
Orange and red means that the [[Lightmap]] resolution is too high.
A selected object is pink instead of its resolution color.

Some meshes may show up in a plain color without any grid pattern.
Even if the exact same [[Static Mesh Asset]] used in another [[Static Mesh Component]] in another [[Actor]] does get the grid pattern.
I do now know why this happens.

# References

- [_Becoming an Environment Artist in Unreal Engine_ > _Lighting_ by Epic Online Learning @ dev.epicgames.com/courses 2020 UE 4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/oE6/unreal-engine-lighting)
