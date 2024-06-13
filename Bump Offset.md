Bump Offset and [[Parallax Occlusion Mapping]] are ways of adding sub-triangle geometry detail.
In that sense they are similar to [[Normal Map]].
They are cheaper than [[Tesselation]], which adds new triangles based on the [[Height Map]].

Bump Offset is typically lower quality than [[Parallax Occlusion Mapping]],
it produces more tearing at sudden changes in height.

Input is a [[Height Map]].
A single-channel texture where small values mean recessed and high values mean extruded.
The height map describe a displacement of fragments when rendering the mesh.
The [[Texture]] used as a height map should have the [[Texture Compression]] settings set to Masks and [[sRGB]] disabled.

The Bump Offset [[Material]] node takes a height input and produces a UV coordinate.
Its function is to compute a new texture location that when the current fragment is rendered as-if it was at the offset location will make it appear as if a flat surface is deformed according to the height map.
The height input is a [[Texture Sample]] from the [[Height Map]] [[Texture]] taken at the fragments default texture coordinate.

If you have the [[Height Map]] packed into a masked [[Texture]] than you need to sample that texture twice.
First with the fragments default texture coordinate to get the height offset for that fragment to pass to Bump Offset, and then a second time with the texture coordinate produced by the Bump Offset node to get the rest of the data packed into the other channels of the [[Texture]].

The Height Ratio input control the scale of the [[Height Map]].
I assume it is in centimeters, but not sure.
A larger Height Ratio makes the offset more pronounced.
This input if often bound to a [[Material Parameter]].

Bump Offset does not generate new geometry and does not move the fragment,
it is in in-triangle visual effect only.
Only small offsets can be made before artifacts starts to appear.
Especially when the [[Height Map]] contains sudden changes between small offsets and large offsets.


# References

- [_An In-Depth Look at Environment Artist Based Tools_ > _Parallax and Bump Offset_ by Epic Online Learning @ dev.epicgames.com 2021 UE4.27](https://dev.epicgames.com/community/learning/courses/3G/unreal-engine-an-in-depth-look-at-environment-artist-based-tools/wpz/unreal-engine-parallax-and-bump-offset)
