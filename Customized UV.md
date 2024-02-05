Part of a [[Material]], has to do with [[Texture]] and [[Texture Sample]].
Shows up as one or more additional input pins on the [[Material Output Node]].
Named Customized UV0, Customized UV1, and so on up to 7.
Set [[Material Editor]] > [[Details Panel]] > Material > Num Customized UVs to the number of custom UVs you want.

Customized UV0 is the regular UV channel.
Modifying Customized UV0 will cause any [[Texture Sample]] node in the [[Material]] that does not have anything connected to the UVs input pin to use the Customized UV0 value.

The Customized UV channels works as a [[Vertex Interpolator]].
The [[Vertex Shader]] evaluates the Customized UV inputs and the GPU interpolates those values over the triangle and passes the interpolated value to the [[Pixel Shader]].

The [[Vertex Interpolator]] is a way to transfer work from the [[Pixel Shader]] to the [[Vertex Shader]].
Calculations that would have been a per-pixel operations,
there are a lot of pixels,
become per-vertex operations,
there are often not as many vertices.
Then we only need to pay for a very cheap interpolation to get the per-pixel values.
This can act as an optimization.
This may cause some rendering artifacts since per-vertex computation and interpolation doesn't necessarily produce the same result as a per-pixel calculation.

Note that high-poly models, such as [[Nanite]] meshes, the usual few-vertices-many-pixels assumption may not hold.
Even for regular non-[[Nanite]] models if the object is far away and only covers a few pixels on screen then there may be more vertices than pixels even for a low-poly mesh.
Though aggressive [[Mesh LOD]] can help.

# References

- [_Materials Master Learning_ > _Additional Features_ by Epic Games @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/KVe/additional-features)
