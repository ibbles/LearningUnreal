#Material 

The Material Editor is where we define a [[Material]].
It contains a Preview panel, a Details panel, and a Material Graph editor.
The Preview panel shows what the material looks like on a sample mesh.
Click Apply in the toolbar to publish the changes made to the Material to all meshes using it.

The Material Graph ends in an output node.
The values we pass to the input pins on the output node define the look of the objects that use this material.
Which input pins are active depends on the settings set in the Details panel.

When rendering the Material Graph is executed on the GPU.
It is run multiple times for every object.
Some output node input pins are evaluated per pixel, others per vertex.

The Material graph is different from the Blueprint Visual Script graph in that it only has expression nodes, no execution nodes.
Another way of looking at it is that the only execution node is the output node.
Each input pin on the output node is connected to a network of nodes branching from that input pin.
Each branch ends with a **value node**, also called an **input node**.
(
"Input node" may have a more specific meaning.
)
A value node can be a literal node, a parameter node, a texture sample node, and probably more.

Nodes are created by right-clicking in the Material Graph editor and searching for the name of the node.
Some nodes have shortcuts.
A **literal node** can be created by holding one of the number keys from `1` to `4` on the keyboard and clicking.
A **Texture Sample** node is created by dragging a texture from the Content Drawer into the Material Graph editor.
Texture Sample nodes use the texture coordinates on the rendered mesh to know where on the texture to sample.
Unless explicit UVs are passed to the Texture Sample node.
A **Vertex Color** node reads the vertex color data from the Static Mesh being rendered.

