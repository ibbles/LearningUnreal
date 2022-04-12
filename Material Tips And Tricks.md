Use a **TexCoord** node, called Texture Coordinate in the library, to zoom in or out of a texture.
Set U- and V Tiling larger than 1 to zoom out, make the pattern smaller, repeat more.
Set U- and V Tiling smaller than 1 to zoom in, make the pattern larger, fewer repetitions.

**Absolute World Position** can be used to get world aligned textures.
Use a Mask node with R and G checked to get only the X and Y coordinates.
Multiply or divide the resulting 2D vector to control tiling.
Pass the 2D vector to a Texture Sample node.
You can use different mask settings to get different projection directions.
Possibly also combine with **VertexNormalWS**, WS stands for World Space, to pick or blend between projection directions.
There is also the  **World Aligned Normal** node, which seems related.
One can also mask out only the Z component of Vertex Normal WS and use that to lerp between two textures.

We can make **color variations** by having a gray-scale texture for patterns and using a **Blend Overlay** node along with a color parameter to chose a hue.
