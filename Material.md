#Material

A Material defines the look-and-feel of an objects.
The function of a Material is defined by its Material Graph, which is constructed in the [[Material Editor]].
The Material has a bunch of inputs.
Some, such as texture coordinates and vertex colors, come from the mesh being rendered.
Some, such as camera position and time, come from the engine itself.
Some, the [[Material Parameter|Material Parameters]] are user provided.
The author of the Material creates the parameters and implement their semantics through the Material Graph.

A Material can have parameters and we can create a [[Dynamic Material Instance]] that set these parameters to particular values.
Parameters are created by adding a Constant node, created by holding 1 to 4 and clicking in the Material Graph, right-click the Constant node and select Convert To Parameter.
Give the parameter a name.

The main part of a Material is the output node.
The values passed to the input pins of the output node define the function of the Material.
Depending on the settings set in the Details panel different input pins are available.

- Base Color: The color of the material.
- Metallic:
- Roughness:
- Anisotropy:
- Emissive Color:
- Opacity:
- Opacity Mask:
- Normal
  Control which direction the surface is facing. I'm not sure if this is in world space or model space.
  Creates surface structures and unevenness that is smaller than a triangle.
  Often read from a texture called a normal map.
- Tangent:
- World Position Offset:
- Subsurface Color:
- Custom Data 0:
- Custom Data 1:
- Ambient Occlusion:
- Refraction:
- Pixel Depth Offset:
- Shading Model:

# Details panel
The following properties are available in the Details panel of the [[Material Editor]].

## Material
- Blend Mode
  Control how this Material with interact with other objects being rendered behind it.
  **Opaque** means that this material will not blend, the pixel value is used as-is regardless of what's behind it.
  **Masked** means that a pixel will either be opaque or discarded. Used to creates shapes smaller than the triangles. Common with foliage.
  **Translucent**: We get gradual opacity, can set it 0..1 per pixel.
- Advanced > Fully Rough: boolean
  Max out roughness, remove any glossiness from the materials.
  Not sure how this relates to the Roughness output, which is still enabled even after setting Fully Rough to true.
  The pin value seems to be ignored.
- Use Material Attributes
  Collapse all the input pins on the [[Material Output Node]] to a single pin. This is useful when you are passing entire material setups around through [[Material Function|Material Functions]], mixing layers and such.
