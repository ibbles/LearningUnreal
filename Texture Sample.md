In an abstract sense, a texture sample is a read from a [[Texture]].
It is kind of like a 2D array look-up, but with floating-point instead of integer array indices,
and the value read may be a mixture of multiple array elements, called the texels of the [[Texture]].
For a texture sample the location to read is called a texture coordinate or a UV coordinate.
This means that we may read between texels.
Different filtering modes determine what happens in this case, how the nearby texels are blended into a single color value.

In Unreal Engine, a Texture Sample is a node in a [[Material Graph]].
The UV coordinate is an input to the Texture Sample node.
By default the UV coordinate is formed by blending the UV coordinates of the three vertices that form the current triangle.

We can get that 2D value, to do some processing on it before passing it to the UV input pin, using the Texture Coordinate node.
The node created is named Tex Coord instead of Texture coordinate.
The Tex Coord node has U Tiling and V Tiling properties to control whether the texture should be scaled down or up.
More tiling means more repetitions of the texture per unit length.

It is important that the [[Details Panel]] > Material Expression Texture Base > Sampler Type matches the [[Texture Compression]] setting set in the [[Texture Editor]] for the sampled texture.
If you set a texture directory on the Texture Sample node then Unreal Editor will make sure this is correct.
If the [[Texture]] to use is a [[Material Parameter]] then you need to keep track of this yourself, though you may get a warning about it if the settings mismatch.

A [[Material]] can contain no more than 16 texture samplers,
some of which, around 3, are already reserved for internal use by the engine.
Such as [[Lightmap]], or [[Shadow Maps]].
However, some graphics APIs, such as DirectX11 and 12, support Shared Texture Samplers.
This increases the limit from 16 to 128.
Enabling shared sampling is done on the Texture Sample node in the [[Material Graph]].
Select the Texture Sample node > [[Details Panel]] > Material Expression Texture Sample > Sampler Source.
Select Shared: Wrap or Shared: Clamp to enable shared sampling for the selected Texture Sample node.

The number of texture samplers used by a [[Material]] is shown in [[Texture Editor]] > Stats panel after the [[Material]] has been compiled.

Another way to reduce the number of required texture samplers in a [[Material]] is to pack multiple single-channel textures into the RGBA channels of a single texture.
For example, we can have alpha channels for four different grass textures in a single texture.
Useful if there is no base color [[Texture]], the grass is just solid green or the color is generated procedurally.
Also, since [[Texture Compression|alpha is not compressed]] but the color channels are, it may be worthwhile to take three alpha textures and put them into the RGB channels of an alpha-less [[Texture]].
Combining different data in a single [[Texture]] like this is sometimes called "mergemaps".

A Texture Sample can be combined with the Bump Offset node to give the illusion of depth in the surface.
It produces a parallax effect.
Connect a Texture Sample from a [[Height Map]] into the Height input pin of the Bump Offset node to control the contour of the imaginary bumps.
The output of a Bump Offset node is a tweaked UV coordinate that should be passed to the regular Texture Sample nodes,
the ones used for Base Color, Roughness, and so on.
Can be used to make it appear like something is behind a glass pane, for example an old CRT TV or a spot-light inside the ceiling or an aquarium.

An even more powerful variant of parallax effect is the Parallax Occlusion Mapping node.
Give it a [[Height Map]] and it produces
- updated UVs to use when sampling color, normal, etc [[Texture|Textures]].
- A shadow mount value to be multiplied into the base color expression to get shadows.
- A pixel depth offset to connect to the Pixel Depth Offset input pin on the [[Material Output Node]].


# Texture Mapping

Texture mapping is the act of assigning texels in a [[Texture]] to points on a [[Mesh]].
Normally, each vertex of the [[Mesh]] have a texture coordinate that identifies a point on the [[Texture]],
and those texture coordinates are interpolated over the triangle formed from the vertices.
The texture coordinates are typically assigned in a mesh modeling software such as Blender.
The texture coordinates goes into the UV input pin of a Texture Sample node.

Sometimes we want to do something custom.
Perhaps we have a [[Mesh]] that lack texture coordinates, or perhaps we are after some other type of effect.
In these cases we can generate, calculate, texture coordinate within the [[Material Graph]].
There are a number of nodes that can help with this.

World Coordinate 3-Way does a planer projection from all three primary axes.
It uses Absolute World Position and the vertex normal to generate a texture sample that is a regular texture lookup for triangles facing one of the primary axes and blend between them for angled triangles.
This gives a seamless and reasonable looking texture wrapping without needing per-vertex texture coordinates.


# References

- [_Materials Master Learning_ > _Mipmaps, Texture Sizes, and Texture Pool_ by Epic Games, Sjoerd de Jong @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/1Yno/unreal-engine-mipmaps-texture-sizes-and-texture-pool)
- [_Becoming an Environment Artist in Unreal Engine_ > _Basic Material Creation and Application_ by Epic Online Learning @ dev.epicgames.com/courses 2020 UE4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/Ya6/unreal-engine-basic-material-creation-and-application)


