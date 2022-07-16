#Material 

The Material Editor is where we define a [[Material]].
It contains a Preview panel, a Details panel, and a Material Graph editor.
The Preview panel shows what the material looks like on a sample mesh.
Click **Apply** in the toolbar to **publish the changes** made to the Material to all meshes using it.

The Material Graph ends in an **output node**.
The values we pass to the input pins on the output node define the look of the objects that use this material.
Which input pins are active depends on the settings set in the Details panel when the output node is selected.

When rendering the Material Graph is executed on the GPU.
It is run multiple times for every object.
Some output node input pins are evaluated per pixel, others per vertex.
I don't know which are per pixel and which are per vertex.

The Material graph is different from the Blueprint Visual Script graph in that it only has expression nodes, no execution nodes.
Another way of looking at it is that the only execution node is the output node.
Each input pin on the output node is connected to a network of nodes branching from that input pin.
Each branch ends with a **value node**, also called an **input node**.
(
"Input node" may have a more specific meaning.
)
A value node can be a literal node, a [[Material Parameter|Parameter node]], a texture sample node, and probably more.

Nodes are created by right-clicking in the Material Graph editor and searching for the name of the node.
Some nodes have shortcuts.
A **literal node** can be created by holding one of the number keys from `1` to `4` on the keyboard and clicking.
Double-click 3- or 4-component literal node to select a color.
A **Texture Sample** node is created by dragging a texture from the Content Drawer into the Material Graph editor.
Texture Sample nodes use the texture coordinates on the rendered mesh to know where on the texture to sample.
Unless explicit UVs are passed to the Texture Sample node.
A **Vertex Color** node reads the vertex color data from the Static Mesh being rendered.

Reroute nodes can be used to tidy up the wires.
Double-click on a wire to create a reroute node.
There are also named reroute nodes.
Named reroute nodes act like teleports, sending a wire's value from one part of the Material Graph to another without the need for a wire between them.
Create a named reroute node with right-click > Add Named Reroute Declaration Node.
Read the value by right-click and search for the name of the named reroute node, or expand Named Reroutes.
Named reroute nodes can have a color, making them easier to tell apart.

You can **zoom** in an out of the Material Graph with the scroll wheel.
Hold Ctrl to zoom closer than 1:1.

The **Material Defaults** panel lists all the parameters in the [[Material]] and their default values.
