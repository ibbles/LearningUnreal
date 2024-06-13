A normal map is a [[Texture]] containing per-pixel normals.
They allow us to get the illusion of a high-resolution mesh using a low-resolution mesh.
They are stored in local coordinates at the location of the sample.
This means that all-blue, i.e. a normal pointing along the Z axis, means that the normal is unaffected, still points straight out of the triangle surface no matter what direction the surface has on the model.
As the color deviates from blue the surface at the pixel is lit as-if the surface normal was tilted away from the actual triangle surface normal.

The intensity of a normal map can be changed by multiplying the red and green channel by some value, (Same value?) but leave the blue channel unchanged.
If the second operand is a constant node then set the RGB values to `[a, a, 1.0]`.

A [[Texture]] used as a normal map should have the following settings in the [[Texture Editor]]:
- Compression > Compression Settings: Normalmap.
- sRGB: Off.
- Texture > Advanced > Flip Green Channel: Depends. Different DCC tools generate the normals differently. DirectX and OpenGL does it differently, and Unreal Engine uses the DirectX convention. Try flipping it if things look wrong. This setting only matters for normal maps, any other type of [[Texture]] should have Flip Green Channel turned off.

# Normal Intensity

We can control the intensity of the normal map from our [[Material]].
The Flatten Normal node can be used for this.
Flatten does the opposite of what we want, so a 1-x node can be placed between a Normal Intensity scalar [[Material Parameter]] and the Flatness input of the Flatten Normal node.
Setting the Normal Intensity [[Material Parameter]] to 1.0 will cause 1-1.0 = 0.0 to be passed to Flatten Normal and the normal read from the normal map will be passed through unchanged.
Setting the Normal Intensity to 0.5 will cause 1-0.5 = 0.5 to be passed to Flatten Normal and the normal will be flattened a bit.
Setting the Normal Intensity to 1.5 will cause 1-1.5 = -0.5 to be passed to Flatten Normal and the normal will be inverse-flattened a bit, causing them to be more intense instead of less.
Weird things will happen if you set Normal Intensity less than 0.0.
1-(-0.5) = 1.5 and Flatten Normal will flatten it more than completely flat, the normal will start to become inverted.

# Blending Normals

If you have multiple normal maps then you should use the Blend Angle Corrected Normals [[Material]] node to blend them.

That node does have an Alpha input pin so I don't know how to set a blend weight.
(
What do I mean by this?
The [example video](https://dev.epicgames.com/community/learning/courses/3G/unreal-engine-an-in-depth-look-at-environment-artist-based-tools/RmB/unreal-engine-controlling-normals) (03:58) only shows Base Normal and Additional Normal input pins.
)

A use-case for blending normals is if you have a full-mesh normal map that approximates a high-resolution source mesh on a low-resolution real-time rendering mesh and also a tileable normal map that provides small-scale surface patterns / texture on the surface.
When having a tileable texture added like this it is common to provide a tiling parameter so the size of the small-scale surface features can be controlled from a [[Material Instance]].
Pass a TexCoord node and a Tileable Scale [[Material Parameter]] to a Multiply Node and pass that to the UVs input pin on the Texture Sample node sampling the tileable texture.

If you have a [[Material Attributes]] wire and a Normal Map you want to blend into the [[Material Attributes]] then you can use a Mat Layer Blend Baked Normal node.
Connect the [[Material Attributes]] into the Material input pin and the Normal Map [[Texture Sample]] into the Normal input pin.

# References

- [_An In-Depth Look at Environment Artist Based Tools_ >  _Controlling Normals_ by Epic Online Learning @ dev.epicgames.com/courses 2021 UE4.27](https://dev.epicgames.com/community/learning/courses/3G/unreal-engine-an-in-depth-look-at-environment-artist-based-tools/RmB/unreal-engine-controlling-normals)
