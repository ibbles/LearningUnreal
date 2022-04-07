#Material

A Material defines the look-and-feel of an objects.
The function of a Material is defined by its Material Graph, which is constructed in the [[Material Editor]].

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

