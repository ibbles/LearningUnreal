The Material Editor is where we define a [[Material]].
It contains a Preview panel, a Details panel, and a [[Material Graph]] editor.
Click **Apply** in the toolbar to **publish the changes** made to the Material to all meshes using it.


# Main Material Node

The Material Graph ends in an **output node** which is called the Main Material Node.
The values we pass to the input pins on the output node define the look of the objects that use this material.
Which input pins are active depends on the settings set in the [[Details Panel]] when the output node is selected.
When a new [[Material]] is created the [[Material Graph]] will contain the Main Material Node only.

# Material Graph

When rendering the [[Material Graph]] is executed on the GPU.
It is run multiple times for every object.
Some output node input pins are evaluated per pixel, others per vertex.
I don't know which are per pixel and which are per vertex.

The Material graph is different from the Blueprint Visual Script graph in that it only has [[Expression node|Expression Nodes]], no [[Execution node|Execution Nodes]].
Another way of looking at it is that the only execution node is the output node.
Each input pin on the output node is connected to a network of nodes branching from that input pin.
Each branch ends with a **value node**, also called an **input node**.
(
"Input node" may have a more specific meaning.
)
A value node can be a literal node, a [[Material Parameter|Parameter node]], a texture sample node, and probably more.

Nodes are created by right-clicking in the Material Graph editor and searching for the name of the node.
Some nodes have shortcuts.

A **constrant vector node** can be created by holding one of the number keys from `1` to `4` on the keyboard and clicking.
Double-click 3- or 4-component literal node to select a color, or edit from the [[Details Panel]].

A **[[Texture Sample]]** node is created by dragging a texture from the Content Drawer into the Material Graph editor,
or by selecting Texture Sample from the right-click menu, or by holding T while left-clicking the [[Material Graph]].
When created from the right-click menu the Texture Sample node will read from the [[Texture]] that is selected in the [[Content Browser]].
Texture Sample nodes use the texture coordinates on the rendered mesh to know where on the texture to sample.
Unless explicit UVs are passed to the Texture Sample node.
There is a limit to the number of texture samplers there can be in a material,
around 13 depending on what [[Shader|Shader Templates]] the [[Material]] depend on.

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


# Details Panel

This is where you control what type of material this is, what features it supports, and such.


# Preview Panel

The Preview panel shows what the material looks like on a sample mesh.
Click on of the buttons in the lower-right to select a mesh to preview the [[Material]] on.
The tea-pot button uses the [[Static Mesh Asset]] that is currently selected in the [[Content Browser]].

Hold L+LMB and move the mouse to change the direction of the incoming light.

Instead of previewing the final material you can preview the output of any node in the graph.
Right-click a node and select Start Previewing Node.
The node will be colored Blue and the text Previewing will be displayed under the node's title.
To stop previewing the node and show the final material again, right-click the node and select Stop Previewing Node.

# Material Graph Editor

Hold RMB and drag to pan the graph.
Scroll-wheel to zoom.
Click and drag between and input pin and and output pin, or vice versa, to connect them.
Hold Alt and click on a pin to break all connections from that pin.


# References

- [_Material Editor Fundamentals for Game Development_ > PBR Properties and the Material Editor by Epic Games, Lincoln Hughes @ dev.epicgames.com 2021](https://dev.epicgames.com/community/learning/courses/pm/unreal-engine-material-editor-fundamentals-for-game-development/PZb/unreal-engine-pbr-properties-and-the-material-editor)
- [_Materials Master Learning_ > _Material Editor Intro and Learning Strategies_ by Epic Games, Sjoerd de Jong @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/oVv/material-editor-intro-and-learning-strategies)
- [_Becoming an Environment Artist in Unreal Engine_ > _Basic Material Creation and Application_ by Epic Online Learning @ dev.epicgames.com/courses 2020 UE4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/Ya6/unreal-engine-basic-material-creation-and-application)

