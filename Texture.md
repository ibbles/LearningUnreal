A texture is an image, often copied to the GPU's memory, VRAM.
Each texture appear as an [[Asset]] in the Content Drawer.
Textures can be imported using the Import button in the Content Drawer.
Textures can be used for many different kinds of data, but there are a few that are very common.

- Diffuse
    - The base color of a surface.
    - Sometimes called Albedo.
- Normal
    - Provide sub-triangle surface unevenness to a surface.
    - Texture containing normals are called "normal maps".
    - Textures containing normals are often sufficed with `_N`.
    - Due to coordinate system differences sometimes imported normal maps are backwards with shadows showing up on the wrong side of a bump or dent. Fix this by checking Flip Green Channel in [[Texture Editor]] > Details panel > Texture.
- Specular
- Roughness

**Scalar data**, such as specular, roughness, and ambient occlusion, can be stored in **different channels** of the same texture.
For example,  the red channel can contain specular data and the green channel roughness data.
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

# Animation Texture

A texture may contain an **animation** by dividing the texture into a grid and put each frame of the animation into a grid cell.
Selecting a frame to show is done by manipulating the UVs, i.e. the texture coordinates, passed to the [[Texture Sample]] node.
The **Flip Book** node helps with this.


# Texture size

Each texture has a size expressed as a two-dimensional value, such as 1024x1024 or 2048x4096.
The size is often a power of two, for performance reasons and for mip-map generation.
It the texture is not a power of two then you can ask Unreal to pad it in [[Texture Editor]] > Details panel > Texture > Power Of Two Mode.

A single texture often contains different versions of itself at different sizes.
This hierarchy is called a mip-map.
It works much like [[Mesh LOD]], but for a texture instead of a [[Static Mesh]].
When a rendered object is far away then we don't need the large sized texture, so the renderer will only keep a smaller version of the texture loaded.
The number of mips generated for a Texture can be seen in the [[Texture Editor]], in the info text shown at the top of the Details panel.

A newer variant of mip-map is [[Virtual Texture]].


# Texture Streaming

[[Texture Streaming]]


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

# References

- [_5 Tips to Optimize Environments in Unreal Engine 4_ - Texture Size by Jakub Haluszczak @ youtube.com 2021](https://youtu.be/gZkKcaF4Ifk?t=386)
- [_Materials Master Learning_ > _Mipmaps, Texture Sizes, and Texture Pool_ by Epic Games, Sjoerd de Jong @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/1Yno/unreal-engine-mipmaps-texture-sizes-and-texture-pool)

