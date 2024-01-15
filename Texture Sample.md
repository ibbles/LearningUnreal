In an abstract sense, a texture sample is a read from a [[Texture]].
It is kind of like a 2D array look-up, but with floating-point instead of integer array indices.
For a texture sample the location to read is called a UV coordinate.
This means that we may read between texels.
Different filtering mode determines what happens in this case, how the nearby texels are blended into a single value.

In Unreal Engine, a Texture Sample is a node in a [[Material Graph]].
The UV coordinate is an input to the Texture Sample node.
By default the UV coordinate is formed by blending the UV coordinates or the three vertices that form the current triangle.

We can get that 2D value, to do some processing on it before passing it to the UV input pin, using the Texture Coordinate node.
The node created is named Tex Coord instead of Texture coordinate.
The Tex Coord node has U Tiling and V Tiling properties to control whether the texture should be scaled down or up.
More tiling means more repetitions of the texture per unit length.
