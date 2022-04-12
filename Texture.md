A texture is an image, often copied to the GPU's memory.
Each texture appear as an [[Asset]] in the Content Drawer.
Textures can be imported using the Import button in the Content Drawer.
Textures can be used for many different kinds of data, but there are a few that are very common.

- Diffuse
  The base color of a surface.
  Sometimes called Albedo.
- Normal
  Provide sub-triangle surface unevenness to a surface.
  Texture containing normals are called "normal maps".
  Textures containing normals are often sufficed with `_N`.
- Specular
- Roughness

**Scalar data**, such as specular, roughness, and ambient occlusion, can be stored in **different channels** of the same texture.
For example,  the red channel can contain specular data and the green channel roughness data.
It is also possible to do more complicated packing, where values are mixed over the channels.

Textures are **sampled**, which means that a value is read from a coordinate on it.
This is often done in a [[Material]], created in the [[Material Editor]].

Each texture has a bunch of metadata that is set in the Details panel of the Texture Editor.
Different **compression settings** are suitable for different types of data.
For example, normal maps should have the `Normalmap` compression setting.

A texture may contain an **animation** by dividing the texture into a grid and put each frame of the animation into a grid cell.
Selecting a frame to show is done by manipulating the UVs, i.e. the texture coordinates, passed to the [[Texture Sample]] node.
The **Flip Book** node helps with this.


(
TODO: Write about sRGB. Once I understand it.
)