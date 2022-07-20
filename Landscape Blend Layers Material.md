Landscape Materials can blend between different layers.
This is done with the Landscape Layer Blend node.
Selecting Landscape Layer Blend from the Add Node list will add a node named Layer Blend.

The assignment of weights of the different layers of a Landscape Material to a particular coordinate on the Landscape is done with [[Paint Material on a Landscape|Landscape Painting]].
The available layers created in the Landscape Layer Blend node  will appear in the Paint tab of the [[Landscape Mode]].
Each layer must have a Layer Info for a particular [[Landscape]] before it can be used.
Create a Layer Info by clicking the `+` next to the layer in the Layers category in the Landscape panel.

The Details Panel of the Landscape Layer Blend node has an array named Layers.
Each entry in this array allows for one more layer in the blending.
Each entry has the following properties:
- `Layer Name`: The name of the layer.
Something to help you identify which layer is which.
- `Blend Type`: Control what decide how strong this layer is at a particular coordinate.
One of `Weight`, `Alpha`, and `Height`. Weight is what we paint with [[Paint Material on a Landscape|Landscape Painting]]. I don't know what Alpha is. Height is the height of the [[Landscape]] at the rendered point, useful for making environment auto materials. I currently don't know how to assign heights to layers.
- `Preview Weight`: Is only used for the preview in the [[Material Editor]].

The Layer Blend node takes one input per layer and will blend between them according to the blend rules set for that Layer Blend node.
The input can, for example, be a color or a normal from a Texture Sample node, or an entire material setup created with a Make Material Attributes node, often the return value from a [[Material Function]].

It is common to have multiple Layer Blend nodes in a Landscape Material.
I assume it is common to want to have the same Layer Blend properties on multiple Layer Blend nodes, but I do not know how to have parameter collections that are applied to all of them.
