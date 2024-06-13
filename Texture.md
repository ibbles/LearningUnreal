A texture is an image, either 2D or 3D, with pixel information.
Textures are often copied to the GPU's memory, VRAM.
Textures are often used in a [[Material]].
A texture may contain information such as color, normal, roughness, metalness, ambient occlusion, and other data, possibly even a combination of those, used by the [[Shading Model]].
Each texture appear as an [[Asset]] in the Content Drawer.
Textures can be imported using the Import button in the Content Drawer.
Textures can be used for many different kinds of data, but there are a few that are very common.

- Base Color, a.k.a. diffuse, albedo, or tint. (RGB value (linear or not?))
    - The base color of a surface.
    - Should not contain and shadow or specular information, just a plain unshaded color.
	    - Unless for a specific reason, e.g. to achieve a particular style.
	- Should use the default [[Texture Compression]] setting.
- Normal (normalized vector)
    - Provide sub-triangle unevenness to a surface.
    - Affects how [[Lighting]] interacts with the surface.
    - The RGB colors channels of a normal texture is a vector describing the direction the surface is angled relative to the triangle.
    - A value of (0.0, 0.0, 1.0), i.e. the Z axis, is normal to the triangle surface.
    - Texture containing normals are called "normal maps".
    - Normal maps are often mostly blue-ish.
    - Textures containing normals are often sufficed with `_N`.
    - Due to coordinate system differences sometimes imported normal maps are backwards with shadows showing up on the wrong side of a bump or dent. Fix this by checking Flip Green Channel in [[Texture Editor]] > Details panel > Texture.
    - Should use the Normal Map [[Texture Compression]] setting.
    - See also [[Normal Map]].
- Specular (0.0 .. 1.0)
- Roughness (0.0 .. 1.0)
	- Controls how rough or smooth a surface is.
	- A high roughness reflects light in all directions.
	- A low roughness reflects light in a narrow cone.
	- A low roughness produces sharp specular highlights, and possibly mirror-like reflections depending on the metalness value.
- Metalness (0.0 or 1.0)
	- How metal-like the surface is.
	- Usually either 0.0 or 1.0, rarely anything in between.
	- Can be in-between in transition areas between a clean metal surface and one covered by e.g. dirt.
- Ambient Occlusion (0.0 .. 1.0)
	- Approximation of less light hitting a surface due to nearby geometry.
	- Parts of the surface that is "in the open" is white.
	- Parts of the texture that is in crevices, corners, or indentations are darker.
	- Most ambient occlusion maps are mostly white.

**Scalar data**, such as specular, roughness, and ambient occlusion, can be stored in **different channels** of the same texture.
For example,  the red channel can contain specular data and the green channel roughness data.
This is called a channel packed texture.
A texture has up to four channels: red, green, blue, and alpha.
This is useful because graphics hardware, and game engines, have a limited number or textures that can be used per [[Material]].
See [[Texture Sample]].
It is also possible to do more complicated packing, where values are mixed over the channels.

Textures are **sampled**, which means that a value is read from a coordinate on it.
This is often done in a [[Material]], created in the [[Material Editor]].

Each texture has a bunch of metadata that is set in the Details panel of the [[Texture Editor]].
Different **compression settings** are suitable for different types of data.
For example:
- Base color textures should have the `Default` setting.
- Normal maps should have the `Normalmap` setting.
- Textures that contain multiple unrelated things, such as red=metallic; green=specular; blue=ambient occlusion, must be set to `Masks`.
	- I think the same should be done even for single-channel textures.
	- So what is the list of things that should be set to `Masks`?


(
[[TODO]] Write about sRGB. Once I understand it.
)
sRGB should be:
- on for base color textures.
- off for normal maps.

A Texture has a group.
A bunch of project-wide settings operate on all Textures with these groups.
A Texture's group is set at [[Texture Editor]] > [[Details Panel]] > Level Of Detail > Texture Group.
There are a bunch of predefined groups such as World, Vehicle, Weapon, Character, Lightmap, and more.
I don't know if it is possible to add custom groups for our project.

Texture group wide settings are at [[Top Menu Bar]] > Window > Developer Tools > Device Profiles.
Select a device and click the wrench button under Texture LOD Groups.
This will bring up a Details panel-like view where Min LOD Size, Max LOD size, LOD Bias and more can be set.


# Texture size

Each texture has a size expressed as a two-dimensional value, such as 1024x1024 or 2048x4096.
The size is often a power of two, for performance reasons and for mip-map generation.
If the texture is not a power of two then you can ask Unreal to pad it in [[Texture Editor]] > Details panel > Texture > Power Of Two Mode.

A single texture often contains different versions of itself at different sizes.
This hierarchy is called a mip-map.
It works much like [[Mesh LOD]], but for a texture instead of a [[Static Mesh]].
When a rendered object is far away then we don't need the large sized texture, so the renderer will only keep a smaller version of the texture loaded.
The number of mips generated for a Texture can be seen in the [[Texture Editor]], in the info text shown at the top of the Details panel.

Each successive level in the mip-map chain is half the size of the previous one in both dimensions.
A 2048x2048 texture has 12 mip-levels, since $2^{(12-1)}$ is 2048.
A 1x1 texture has 0 mip levels, since $2^{1-1}$ is 1.

A newer variant of mip-map is [[Virtual Texture]].

Texture sizes are sometimes expressed using the k suffix.
This is a base-2 k, since textures should have a power-of-two size.
- 1k = 1024
- 2k = 2048
- 4k = 4096
- 8k = 8192

Texture sizes are rarely larger than 2048, though this has increased over time.
Unreal Engine has an upper limit on the texture size, it use to be 8192 but may have been increased to 16384.

# Texture Density

(
I'm not sure if texture density is measured in texels per pixel or texels per unit length.
Does the texture density increase when we move the camera closer to a textured [[Mesh]]?
)
For each texture, pick a texture size based on the expected size of the textured mesh on screen.
This results in a **texture density**, which is the number of texels per pixel.
Also called texel density or pixel density.
For example, a mug often takes up space on screen than a wall so the mug should have a smaller sized texture than the wall.
The texture density is determined by:
- The size of the texture.
- How the texture coordinates of the [[Mesh]] vertices has been set.
- The size of the [[Mesh]].

The texel density can be altered in a [[Material]] by tiling the texture,
i.e. by scaling the texture coordinates.
This only works for tileable textures, not when specific parts of the texture must map to specific parts of the mesh.

A target texture density is decided early in the project by an art director.
The texture density to target depend on the type of game.
A first-person game often require a higher texture density than a top-down game,
since the camera tends to be closer to the geometry in the level.
(
This indicates that texture density is texels per unit length, not texels per pixel.
)
Some objects may be given a higher texture density due to their importance and visibility,
such as the things the player character holds in their hands in a first-person game.

 
# Texture Streaming

[[Texture Streaming]]


# Exporting A Texture From A DCC Tool

Export the image in PNG or TGA format.
Store the image with 8 bits per channel, 24 bits per texel for RGB textures and 32 bits per texel for RGBA textures.
Make the texture size a power of two.
Don't make the texture larger than 8192 or 16384, depending on Unreal Engine version.
(
TODO Check which engine version started supporting 16384 size textures.
)
It is OK to have rectangular textures.

# Creating A Texture In C++

```cpp
UTexture2D RuntimeTexture = UTexture2D::CreateTransient(1024, 1024);
FTexture2DMipMap* MipMap = &RuntimeTexture->PlatformData->Mips[0];
FByteBulkData* ImageData = &MipMap->BulkData;
uint8* RawImageData = (uint8*)ImageData->Lock(LOCK_READ_WRITE);

// Edit RawImageData
// RawImageData is formatted as such:
// [Pixel1 B][Pixel1 G][Pixel1 R][Pixel1 A][Pixel2 B][Pixel2 G][Pixel2 R][Pixel2 A] …

ImageData->Unlock();
RuntimeTexture->UpdateResource();
```


# Animation Texture

A texture may contain an **animation** by dividing the texture into a grid and put each frame of the animation into a grid cell.
Selecting a frame to show is done by manipulating the UVs, i.e. the texture coordinates, passed to the [[Texture Sample]] node.
The **Flip Book** node helps with this.


# References

- [_5 Tips to Optimize Environments in Unreal Engine 4_ - Texture Size by Jakub Haluszczak @ youtube.com 2021](https://youtu.be/gZkKcaF4Ifk?t=386)
- [_Materials Master Learning_ > _Mipmaps, Texture Sizes, and Texture Pool_ by Epic Games, Sjoerd de Jong @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/1Yno/unreal-engine-mipmaps-texture-sizes-and-texture-pool)
- [_Becoming an Environment Artist in Unreal Engine_ > _Texture Settings, Density and Exporting_ by Epic Online Learning @ dev.epicgames.com/courses 2020 UE4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/Zaw/unreal-engine-texture-settings-density-and-exporting)

