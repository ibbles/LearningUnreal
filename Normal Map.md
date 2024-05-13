A normal map is a [[Texture]] containing per-pixel normals.
They are stored in local coordinates at the location of the sample.
This means that all-blue, i.e. a normal pointing along the Z axis, means that the normal is unaffected, still points straight out of the triangle surface no matter what direction the surface has on the model.
As the color deviates from blue the surface at the pixel is lit as-if the surface normal was tilted away from the actual triangle surface normal.

The intensity of a normal map can be changed by multiplying the red and green channel by some value, (Same value?) but leave the blue channel unchanged.
If the second operand is a constant node then set the RGB values to `[a, a, 1.0]`.

A [[Texture]] used as a normal map should have the following settings in the [[Texture Editor]]:
- Compression > Compression Settings: Normalmap.
- sRGB: Off.
- Texture > Advanced > Flip Green Channel: Depends. Different DCC tools generate the normals differently. DirectX and OpenGL does it differently, and Unreal Engine uses the DirectX convention. Try flipping it if things look wrong. This setting only matters for normal maps, any other type of [[Texture]] should have Flip Green Channel turned off.

# Blending Normals

If you have multiple normal maps then you should use the Blend Angle Corrected Normals [[Material]] node to blend them.
That node does have an Alpha input pin so I don't know how to set a blend weight.
