An alternative to [[Landscape Weight Layers Material]].
The two are mostly functionally equivalent, but Landscape Blend Layers Material supports the Height blend type.

When a Blend Layers Material has been assigned to [[Landscape]] > [[Details Panel]] > [[Landscape Material]] the layers created in the Blend Layers Material will show up in the Paint sub-mode of the [[Landscape Mode]],
making it possible to paint that layer onto the [[Landscape]].

It is often a good idea to set [[Material Output Node]] > [[Details Panel]] > Material > Use Material Attributes to true.
To make it easier to blend multiple [[Material]] setups together using a single wire and blend node.

It is often a good idea to create a [[Material Function]] for each layer.
This makes it possible to reduce the layer in multiple [[Material]]s, and reduce the clutter in the main [[Material]].

# Creating a Layer Blend Node

A [[Landscape Material]] can blend between different layers.
This is done with the Landscape Layer Blend node.
Selecting Landscape Layer Blend from the Add Node list will add a node named Layer Blend.

# Creating A Layer

Layers are setup from the Layer Blend node's [[Details Panel]].
The [[Details Panel]] of the Landscape Layer Blend node has an array named Layers.
Add an element to the [[Details Panel]] > Material Expression Landscape Layer Blend > Layers array to create a new layer.

Each entry in this array allows for one more layer in the blending.
Each entry has the following properties:
- `Layer Name`: The name of the layer.
	- Something to help you identify which layer is which.
	- Shown both on the Layer Blend node and in the list of layers in the [[Landscape Painting]] sub-mode of the [[Landscape Mode]].
- `Blend Type`: Control what decide how strong this layer is at a particular coordinate.
	- One of `Weight`, `Alpha`, and `Height`.
	- Weight is what we paint with [[Paint Material on a Landscape|Landscape Painting]].
	- I don't know what Alpha is.
	- Height is the height of the [[Landscape]] at the rendered point, useful for making environment auto materials. Not sure if this is relative to the world origin Z or the zero-height plane of the [[Landscape]]. It blends based on the height value from 0..1, a grayscale value.
- `Preview Weight`: Is only used for the preview in the [[Material Editor]].

The assignment of weights of the different layers of a [[Landscape Material]] to a particular coordinate on the Landscape is done with [[Paint Material on a Landscape|Landscape Painting]].
The available layers created in the Landscape Layer Blend node will appear in the Paint tab of the [[Landscape Mode]].
Each layer must have a Layer Info for a particular [[Landscape]] before it can be used.
Create a Layer Info by clicking the `+` next to the layer in the Layers category in the Landscape panel.

Height Blend can also be controlled with a texture. 
When Height Blend is enabled on a layer we get an extra input pin on the Layer Blend node that is the height.
If we create a [[Texture]] that is a mask of where we want one material or another and connect a [[Texture Sample]] from that texture to the Height input pin the we can control which parts are one material and which parts are the other material.
This can, for example, be used to make rocks appear out of sand.
Have one set of textures for a sand material and another set of textures for a rock-covered ground.
Create a mask texture that is white where there are rocks and black where there are no rocks in the rock textures.
Pass that to the Height input and we will see the rocks where there are rocks and the space between will be filled with the sand material.

The Layer Blend node takes one input per layer and will blend between them according to the blend rules set for that Layer Blend node.
The input can, for example, be a color or a normal from a Texture Sample node, or an entire material setup created with a Make Material Attributes node, often the return value from a [[Material Function]].

It is common to have multiple Layer Blend nodes in a Landscape Material.
I assume it is common to want to have the same Layer Blend properties on multiple Layer Blend nodes, but I do not know how to have parameter collections that are applied to all of them.


# References

- [_Materials Master Learning_ > _Additional Features_ by Epic Games @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/KVe/additional-features)
- [_Advanced Skill Sets for Environment Art_ > _Landscape Materials and Layers_ by Epic Online Learning @ dev.epicgames.com/courses 2022 UE4.27](https://dev.epicgames.com/community/learning/courses/Qwa/unreal-engine-advanced-skill-sets-for-environment-art/2YXB/landscape-materials-and-layers)

