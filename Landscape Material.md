#Landscape
#Material

A Landscape Material is a [[Material]] intended for use on a Landscape.
In particular, a Landscape Material is a Material that contains one or more Landscape Layer Blend nodes.

Landscape Materials are created just like a regular [[Material]].
Content Browser > Right-click > Material.

A Landscape Material can use Texture Sample nodes.
A Landscape Material can include [[Landscape Foliage]].

# Layer Blending
Landscape Materials can blend between different layers.
This is done with the Landscape Layer Blend node.
Selecting Landscape Layer Blend from the Add Node list will add a node named Layer Blend.

The assignment of weights of the different layers of a Landscape Material to a particular coordinate on the Landscape is done with [[Paint Material on a Landscape|Landscape Painting]].
The available layers will appear in the Paint tab of the [[Landscape Mode]].
Each layer must have a Layer Info for a particular [[Landscape]] before it can be used.
Create a Layer Info by clicking the `+` next to the layer in the Layers category in the Landscape panel.

The Details Panel of the Landscape Layer Blend node has an array named Layers.
Each entry in this array allows for one more layer in the blending.
Each entry has the following properties:
- `Layer Name`: The name of the layer.
Something to help you identify which layer is which.
- `Blend Type`: Control what decide how strong this layer is at a particular coordinate.
One of `Weight`, `Alpha`, and `Height`.
- `Preview Weight`: Is only used for the preview in the [[Material Editor]].

The Layer Blend node takes one input per layer and will blend between them according to the blend rules set for that Layer Blend node.
The input can, for example, be a color or a normal from a Texture Sample node, or an entire material setup created with Make Material Attributes.

It is common to have multiple Layer Blend nodes in a Landscape Material.
I assume it is common to want to have the same Layer Blend properties on multiple Layer Blend nodes, but I do not know how to have parameter collections that are applied to all of them.

# Texture coordinates
Tiling of textures is controlled with a Landscape Layer Coords node.
Increase `Mapping Scale` to make the texture bigger. (I think)

[UE4: Step-by-Step to Your First Landscape Material for Beginners (Day 2/3: 3-Day Tutorial Series) @ youtube.com by WorldofLevelDesign](https://www.youtube.com/watch?v=cWOlIvq0Etg)

