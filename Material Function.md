A Material Function is a collection of [[Expression Node|Expression Nodes]] that can be called from a [[Material Graph]].
It is a way to turn many nodes in a [[Material]] into a reusable [[Asset]] that can be called from many Materials, or many places in the same Material.
Makes iterating on such functionality much easier since we don't need to update every [[Material]], only the Material Function.
Collaboration is easier since different people working on different features can work in different assets.
A drawback is that editing a Material Function causes recompilation of all permutations of all Materials that use that function.

Nodes that call a Material Function have a blue title background.
Nodes that call a Material Function are called call nodes.
The node can be double-clicked to open the Material Function, displaying the [[Material Graph]] that defines the behavior of the function.

A new Material Function is created with [[Content Browser]] > right-click > Materials & Textures > Material Function.

In the Material Function's [[Details Panel]] tick the Material Function > Expose To Library checkbox to make the function show up in the [[Material Editor]] lists.
If you can't find a function you know exists, open the function and make sure the Expose To Library checkbox is ticked.

A Material Function can have multiple inputs and outputs,
showing up as input and output pins on the node.


# Input

An input is created by right-click in the [[Material Graph]] and selecting Function Input.
The type of the input is selected in the Function Input's [[Details Panel]].
Can be one of:
- Scalar
- Vector \[234\]
- Texture 2D
- Texture Cube
- Static Bool
- Material Attributes

We can also set a name, description, and sort priority in the [[Details Panel]].

A default value can be set and enabled in the Function Input's [[Details Panel]].
An input with a default value enabled get a gray colored input pin on the call node.
An input without a default value has a white input pin on the call node and must be connected for the [[Material]] to compile.


# Output

An output is created by right-click in the [[Material Graph]] and selecting Function Output.
A Material Function can have multiple output nodes.
The show up as output pins when the Material Function is called from a [[Material]].

In the [[Details Panel]] we can also set:
- Name: Printed next to the pin in a call node.
- Description: show in the tool-tip when hovering over the pin on a call node.
- Sort priority: Defines the order of the pins in a call node. Low priority pins appear above high-priority pins.

The list in the Palette panel of the [[Material Editor]] can be filtered to only show Material Functions.

# References

- [_Materials Master Learning_ > _Material Functions_ by Epic Games @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/108/material-functions)
- [_An In-Depth Look at Environment Artist Based Tools_ > _Material Functions_ by Epic Online Learning @ dev.epicgames.com/courses 2021 UE4.27](https://dev.epicgames.com/community/learning/courses/3G/unreal-engine-an-in-depth-look-at-environment-artist-based-tools/Mop/unreal-engine-material-functions)
