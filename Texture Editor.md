An editor for [[Texture]] settings.
Not the texture data itself, but for mip-map settings, [[Texture Compression]], and things like that.

The [[Texture]] editor consists of two main parts: a viewport where we can see the texture and a [[Details Panel]] where we can see and configure the texture's settings.

The top part of the [[Details Panel]] show some statistics about the [[Texture]], such as its size and maximum in-game size.
The maximum texture size can be set with Details panel > Compression > Advanced > Maximum Texture Size.
The engine will not load a mip level larger than this.
This is one way to limit the texture memory requirement without resizing the source texture data.

[[Details Panel]] > Texture > sRGB control whether or not the texture should be gamma-corrected or not.
(
I think, I don't understand sRGB yet.
)
This should, perhaps, be enabled on color textures, but should be disabled on any texture containing other data such as normals, roughness, or metalness.
sRGB is automatically disabled when selecting the Masks option for Compression Settings.