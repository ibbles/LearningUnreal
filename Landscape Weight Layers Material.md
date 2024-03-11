An alternative to [[Landscape Blend Layers Material]].
The two are mostly functionally equivalent, but [[Landscape Blend Layers Material]] supports the Height blend type.

Create a bunch of Landscape Layer Weight nodes, one for each type of [[Material]] to use on the [[Landscape]].
In each Landscape Layer Weight node's [[Details Panel]], set a name for the layer.
Many Landscape Layer Weight nodes can have the same name as long as they connect to different input pins on the [[Material Output Node]].

Each Landscape Layer Weight node has two input pins:
- Base
- Layer

The Base input pin is used to chain Landscape Layer Weight nodes together.
The output of one Landscape Layer Weight node goes into the Base input pin of the next Landscape Layer Weight node.
The first Landscape Layer Weight node has the Base input pin unconnected.
The last Landscape Layer Weight node has the output pin connected to one of the input pins on the [[Material Output Node]].

The Layer input pin is the value to use when that layer is active.
It is common to read from a [[Texture Sample]].
When sampling a texture in a [[Landscape Material]] you can use the Landscape Coords node instead of the Tex Coord node.
This makes it possible to select projection mode.

There can be many chains of Landscape Layer Weight nodes,
for example one into Base Color and another into Normal on the [[Material Output Node]].
I think one should have the same layer names in the same order in all chains.
Not sure what happens if they are different.

The [[Landscape Painting]] mode detects the Landscape Layer Weight node names and provides them as different "colors" when painting.
That is, we can set the paint tool to paint one of the layers onto the [[Landscape]].
Select the [[Landscape Mode]], switch to the Paint tab, under Target Layers you will see the layers you created in the [[Material]].
Select one and click-and-drag on the [[Landscape]] in the [[Level Viewport]] to activate the selected layer at that location on the [[Landscape]].


# References

- [_Materials Master Learning_ > _Additional Features_ by Epic Games @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/KVe/additional-features)
