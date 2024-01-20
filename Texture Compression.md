[[Texture|Textures]] are compressed because we have limited memory and bandwidth on the graphics card.
Texture compression does not affect memory performance,
unless the lack of compression, or too large textures, causes stuttering and hitches.
Should not affect steady-state median frame times.
Too much texture data will also result in low-resolution blurry textures in the [[Viewport]].

Textures are compressed on import.
There are different types of compression algorithms used for different purposes and platforms.
- BC - Block Compression.
	- Used for DirectX on PC.
	- Has different types identified by a number:
		- BC1: Without alpha channel.
		- BC3: With alpha channel.
			- The colors are compressed as with BC1.
			- The alpha channel is not compressed, it is as large as the three color channels combined.
		- BC5.
		- BC7.
			- Can be selected from the Compression Settings drop-down in the [[Details Panel]].
			- Higher quality than BC1 or 3, but also larger size.
	- Also called DXTC - DirectX Texture Compression, or DXT which is the same thing.
		- The type numbers aren't the same between BC and DXTC.
		- BC1 = DXTC1.
		- BC3 = DXTC5.
		- Don't know about the rest.

Even if the imported texture has an alpha channel, we can omit that from the compressed texture by ticking [[Details Panel]] > Compression > Compress Without Alpha.

Compression can be disabled, but this is rarely done.

Some textures, such as [[Normal Map]] or [[Vector Displacement Map]], does not work when compressed by a color optimized compression algorithm.
There are other compression algorithms for those.
Select compression algorithms for a [[Texture]] at [[Texture Editor]] > [[Details Panel]] > Compression > Compression Settings.
They are named after what they are supposed to be used for.
- Normal Map.
	- Should be used for all normal maps, which is very common to have.
	- Removes the blue channel, uses the saved space to not compress red and green as hard.
	- The generated [[Shader]] code restores the value of the blue channel from the red and green.
	- This is possible since normals are always normalized.
- Vector Displacement Map.
	- Uncompressed.
	- RGBA8 or B8G8R8A8. Not sure which, the Compression Settings says one thing but the Format at the top of the [[Details Panel]] says the other.
	- Doesn't do much with the texture data, it is basically a straight import.
- Grayscale.
	- Not intended to be used for grayscale images, does not do compression.

A recommendation is to be observant of changes to Resource Size at the top of the [[Details Panel]] and react if it behaves in unexpected ways,
e.g. goes up when a grayscale texture is changed from the default texture compression to grayscale.

The [[Statistics Browser]] can tell you the amount of memory used by textures.
`stat memory` shows the usage in-game.

# References

- [_Materials Master Learning_ > _Compression and Memory_ by Epic Games, Sjoerd de Jong @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/Y0q/compression-and-memory)