An [[Editor Mode]] used to assign [[Vertex Color]] to vertices on a mesh.
Used much like any paint program, select a color, a brush size and strength, and click-and-drag over the selected mesh right in the [[Level Viewport]].

From the Mesh Paint panel we can select which channel(s) of the [[Vertex Color]] we want to visualize in the [[Level Viewport]] by setting Color View Mode.
Set Color View Mode to Off to display the [[Mesh]] using its regular [[Material]].
This is useful when a [[Blend Material]] based on [[Vertex Color]] is used.


# Select Sub-Mode

A [[Mesh]] must be selected before any painting can be done.
To select a [[Mesh]], use the Select sub-mode of the Mesh Paint [[Editor Mode]].


# Pant Sub-Mode

To start painting, switch to the Paint sub-mode.
Paint settings will appear in the Mesh Paint panel.

The brush size it set with Brush > Size.
The keyboard shortcut for changing the brush size is `[` and `]`.
The size of the brush is visualized in the [[Level Viewport]] around the mouse cursor.
Vertices within the brush radius are displayed as squares.
The size of the vertex squares are controlled with Mesh Paint panel > Vertex Painting > Visualization  >< Vertex Preview Size.
Remember that [[Vertex Color]] is only painted on the vertices.
It is not possible to paint between the vertices.

Mesh Paint panel > Brush > Strength controls how fast the [[Vertex Color]] changes from the current color towards the paint color.

Mesh Paint panel > Brush > Falloff controls how Strength varies over the brush area.
A low falloff makes the strength be high all the way to the edge of the brush, resulting in a sharp edge.
A high falloff makes the strength reduce more quickly towards the edge of the brush, resulting in a smoother / softer edge.

Mesh Paint panel > Brush > Enable Brush Flow enables click-and-drag painting, as opposed to click-only painting.
(I think, instructions unclear.)

Mesh Paint panel > Brush > Ignore Back-Facing makes it so only vertices on front-facing triangles are painted.
Since back-facing triangles are often culled during rendering, having this on can make it so that you paint on triangles you can't even see, only being noticed when you look at the mesh from the other side.

Mesh Paint panel > Vertex Painting > Paint Color is the color painted when clicking on the mesh in the [[Level Viewport]].

Mesh Paint panel > Vertex Painting  Erase Color is the color painted when holding Shift and clicking on the mesh in the [[Level Viewport]].

Type `x` or hold `Shift` to switch between the foreground and background color.

Setting a color controls the maximum level painting will reach.
This is good to prevent overblown blends by setting a color that is in-between two blended colors.

Mesh Paint panel > Color Painting > Channels controls which channels are being painted into.
Checking only the channel you want to paint is safer than setting a pure Vertex Painting > Paint Color since it makes it possible to use black as the Erase Color.
Consider setting Mesh Paint panel > Visualization > Color View Mode to match Color Painting > Channels.

Mesh Paint panel > Color Painting > LOD Model Painting allows you to select between all [[LOD]] levels or a particular [[LOD]] level.
Most common is to paint to all levels.
A possible use-case if when higher levels, i.e. lower resolution, have a different [[Blend Material]] that blends between fewer layers, in that case you may want a different [[Vertex Color]] setup for those [[LOD]] levels.

# Limitations

Vertex painting cannot be done on [[Nanite]] meshes.

# References

- [_An In-Depth Look at Environment Artist Based Tools_ > _Vertex Paint_ by Epic Online Learning @ dev.epicgames.com/courses 2021 UE4.27](https://dev.epicgames.com/community/learning/courses/3G/unreal-engine-an-in-depth-look-at-environment-artist-based-tools/WvD/unreal-engine-vertex-paint)
