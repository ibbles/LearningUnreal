Parallax Occlusion Mapping and [[Bump Offset]] are ways of adding sub-triangle geometry detail.
In that sense they are similar to [[Normal Map]].
They are cheaper than [[Tesselation]], which adds new triangles based on the [[Height Map]].

Parallax Occlusion Mapping is typically higher quality than [[Bump Offset]],
it produces less tearing at sudden changes in height.

Input is a [[Height Map]].
A single-channel texture where small values mean recessed and high values mean extruded.
The height map describe a displacement of fragments when rendering the mesh.
The [[Texture]] used as a height map should have the [[Texture Compression]] settings set to Masks and [[sRGB]] disabled.

The Parallax Occlusion Mapping [[Material]] node outputs new texture coordinates that should be used when sampling textures.

The Parallax Occlusion Mapping node is given:
- A [[Height Map]] [[Texture]].
- A [[Height Map]] channel.
	- If the height data is in a particular channel then the channel must be identified using the Heightmap Channel input as a 4-component vector that has a 1.0 where the height map data is. I imagine that the Parallax Occlusion Mapping node does a dot product between the texture sample and the Heightmap Channel input.
- A texture coordinate to use to sample the [[Height Map]] [[Texture]].
- A height ratio which scales how much to offset the fragment based on the [[Height Map]] value.
	- Not sure on the unit here, but I think it is in centimeters, i.e. when reading pure white value from the [[Height Map]] the fragment will be offset by Height Ratio centimeters.
- {Max, Min} Steps is how many iteration steps the Parallax Occlusion Mapping algorithm should take to generate the effect. The number of steps takes is determined by the angle at which the surface is viewed from, with glancing angles requiring more steps. Higher values gives better results but comes with a performance cost. Reasonable values are 8 and 32.


# References

- [_An In-Depth Look at Environment Artist Based Tools_ > _Parallax and Bump Offset_ by Epic Online Learning @ dev.epicgames.com/courses 2021 UE4.27](https://dev.epicgames.com/community/learning/courses/3G/unreal-engine-an-in-depth-look-at-environment-artist-based-tools/wpz/unreal-engine-parallax-and-bump-offset)
