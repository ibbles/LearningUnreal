A normal map is a [[Texture]] containing per-pixel normals.
They are stored in local coordinates at the location of the sample.
This means that all-blue, i.e. a normal pointing along the Z axis, means that the normal is unaffected, still points straight out of the surface no matter what direction the surface has on the model.
As the color deviates from blue the surface at the pixel is lit as-if the surface normal was tilted away for the the actual surface.

The intensity of a normal map can be changed by multiplying the red and green channel by some value, (Same value?) but leave the blue channel unchanged.
If the second operand is a constant node then set the RGB values to `[a, a, 1.0]`.


# Blending Normals

If you have multiple normal maps then you should use the Blend Angle Corrected Normals [[Material]] node to blend them.
That node does have an Alpha input pin so I don't know how to set a blend weight.
