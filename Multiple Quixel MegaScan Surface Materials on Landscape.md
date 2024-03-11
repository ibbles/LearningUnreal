This text is based on two examples by MR3D-Dev.
Links are at the bottom.

The goal is to create a Landscape that uses multiple [[Quixel Surface Materials]].
A Landscape can only have a single [[Landscape Material]], but that material can have multiple Layers that are blended together based on [[Paint Material on a Landscape|layer weights painted onto the Landscape]].

Ideally we would reference the Quixel Surface Material directly from our new Landscape Material, but I have not found a way to do that.
Instead we will re-implement the Quixel Master Material in a collection of [[Material Function]], one for each Quixel Surface Material we wish to use on our Landscape, call those functions from our new Landscape Material, and blend the results using Layer Blend nodes.

If you use Quixel Bridge to download assets you must ensure that Enable Displacement is enabled in the Megascans Settings.

# Create Landscape Material
To create a Landscape that contains multiple layers each with a separate Quixel Surface Material create a new [[Material]] that will do the blending.

The Material should contain one or more Landscape Layer Blend nodes.
The node itself only display Layer Blend in the name.
In the Layer Blend node's Details Panel add as many elements in the Layers array as you have Quixel Surface Materials you want to use on the Landscape.

Each array element has the following important properties:
- `Layer Name`:
Give the layer a descriptive name, so users know what this layer is for.
- `Blend Weight`:
Control how this layer is blended with the rest. Can be one of `Weight`, `Alpha`, and `Height`.

Each array element in the Layers property of the Layer Blend node produces an input pin on the node.
Each input will come from another "pseudo-Material", implemented as a [[Material Function]].


# Material Functions re-implementing the Quixel Master Material

Unfortunately, we can't call out to other Materials from our [[Landscape Material]].
Instead we open the Material we want to use, follow the parent chain until we hit a Material that is not a [[Material Instance]] and then copy the relevant Graph Nodes of that Material's Node Graph into a new [[Material Function]].
In the [first example](https://youtu.be/esuOUHfRjsE?t=234) the last node copied into the Material Function is `MF_MapAdjustments`.
In the [second example](https://youtu.be/YGOvGOJo4XY?t=253) the displacement node is copied as well.
The output of `MF_MapAdjustments` goes into the input pin of the `OutputResult` node in the Material Function.
Create one Material Function for each input to the Layer Blend node, each Material Function being named the same as the layer it connects to.
The naming doesn't actually have to match, but it makes things easier. 
Add a call to the new Material Function to the new Landscape Material and connect the output of the Material Function to the corresponding input of the Layer Blend node.

# Material Parameters
When copying Graph Nodes into Material Functions all being called from the same Material we have likely caused parameter shadowing or aliasing.
(
I don't know if the Parameters will shadow each other, or if setting a Parameter name will set all of them, a.k.a. aliasing.
Experimentation needed.
)
Shadowing (maybe?) happens when we have multiple Material Parameters with the same name.
The effect of this is that only one of the equal-named parameters is configurable, or that all of them will always have the same value.
(
Maybe, see note-to-self above.
Still, we want control over individual Parameters so read on.
)

There are two ways to fix this.
One way is to disambiguate the parameters by renaming them.
This approach is shown in the [first example](https://youtu.be/esuOUHfRjsE?t=385) and described under Instanced Material Functions below.
Another approach is to make them not parameters, just set a value at let it be that value.
This approach is shown in the [second example](https://youtu.be/YGOvGOJo4XY?t=229) and under Baked Material Functions below.

## Baked Material Functions
I'm not sure why it is called Baked Material Function since I don't see any baking going on.
Create a Material Function for every Material you want to apply to the Landscape.
Name the Material Function something similar to the Material it is based on.
Copy the nodes from the [[Master Material]] into the new Material Function.
Among the nodes will be a bunch of Parameter nodes.
Rename the parameters to something based on the Material name and assign them all to a group with a similar name.
Assign each texture that came with the Quixel MegaScans Material to the corresponding Param 2D node.
It may be possible to un-parameter the texture nodes, but it's not shown in the example.

## Instanced Material Functions
To fix parameter shadowing/aliasing we need to rename all Material Parameter nodes in the Material Functions we created.
I suggest giving each a prefix that is the name of the layer followed by an underscore.

In addition, we can add the same prefix to the Group property of all Material Parameters.
For example, all texture maps for color and roughness and such can be called `Layer A_Maps`, or something like that.
Preferably with `Layer A` being a more descriptive and use-case appropriate name.

There are also Parameters in the Material Functions being called from the copied nodes.
We could double-click the function call nodes and modify the functions directly, but then we would be modifying the actual Quixel MegaScans assets which we might not want to do.
I recommend making a copy.
In the [first example](https://youtu.be/esuOUHfRjsE?t=651) there are two Material Functions, `MF_Tiling` and `MF_DetailNormalTiling`.
Double click the copied function call node to open the Material Function Editor.
Click Browse to locate the Material Function asset in the Content Browser.
Right-click > Duplicate.
Move the copy out of the MegaScans directory, to any project-specific directory.
Create duplicates of the duplicate until you have one Material Function per layer in your Landscape Material.
Inside each duplicate, rename ever Parameter node, adding a post-fix unique to the layer this duplicate was made for.
Also, in the Parameter's Details Panel, set the Group to something layer-specific.

Drag the new Material Functions from the Content Browser to their respective layer Material Functions.
The ones to which we copied nodes from the Quixel MegaScans Material.
Move all wires from the old/original function call node to the new one using Ctrl+Drag on the output pins.

# Material Output
At the very end of the Landscape Material, between the Layer Blend node and the output node, add a Break Material Attributes node.
Connect the relevant output pins of the Break Material Attributes node into the corresponding input poins of the Material's output node.
You will have to figure out which pins are relevant by inspecting the contents of the Material function that was created above.
Or just connect all the pins, I assume that will work too.


# Displacement
[second example](https://youtu.be/YGOvGOJo4XY?t=870).

Quixel MegaScans may or may not come with displacement support.
If you use the MegaScans plugin for Unreal Editor, check Enable Displacement in the Megascans Settings.
The Quixel MegaScan Materials have a Material Function named `MF_Displacement`.


# Material Instance
Create a [[Material Instance]] of the Landscape Material by Content Browser > your Material > right-click > Create Material Instance.
The Material Instance Editor for your Material Instance will show all the Parameters for all the Materials injected into the Landscape Material.
Assuming you renamed and grouped them properly.

I think the Quixel MegaScans materials are similar enough that one can do a complete switch from one Material to another in a Material Instance by only swapping the texture parameters.
That would be neat.
Then we only need a single Landscape Material per number of layers / input Materials we want to support.
Simply find the layer maps parameter group you created and the textures for the Quixel MegaScans you want to use, and assign each texture to the corresponding texture parameter.

Assign the Material Instance to the Landscape by selecting the Landscape and setting Details Panel > Landscape Material to you Material Instance.
The Landscape will turn black.
This happens because we haven't told the Landscape where each layer should be applied.
The information used by the Layer Blend node.

# Landscape Layers
With a Landscape Material containing Blend Nodes assigned to the Landscape we can see those layers in the Landscape Panel when the Paint Mode is selected, under Target Layers > Layers.
Each layer must have a [[Layer Info]], which is created by clicking the `+` next to each layer.
The Layer Info has a bunch of settings but I don't know what they mean.
To paint a particular material onto the Landscape, select the corresponding layer in the list and click-drag over the Landscape.


[New Megascans Landscape Blend Material in Unreal Engine 4 @ youtube.com by MR3D-Dev ](https://www.youtube.com/watch?v=esuOUHfRjsE)

[Megascan Landscape Blend Material with Displacement in UE4 (2.0) @ youtube.com by MR3D-Dev](https://www.youtube.com/watch?v=YGOvGOJo4XY)

[Fixing Landscape Tiling - Building Worlds In Unreal - Episode 5 @ youtube.com by Ben Cloward](https://www.youtube.com/watch?v=VrQ_rVtH9Zk)

