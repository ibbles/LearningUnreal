A [[Landscape Material]] can trigger the spawning of [[Foliage]].
This is done by adding a **Landscape Grass Output** node to the [[Material]].
The Landscape Grass Output node show up as a Grass node.
The Details Panel of the Grass node has a **Grass Types** array.
This array contains instances of the Landscape Grass Type asset.

**Landscape Grass Type** is an asset type.
A new Landscape Grass Type is created with Content Drawer > right-click > Foliage > Landscape Grass Type.
This is a [[Foliage]] type of asset so it often has the `F_` name prefix.
The Landscape Grass Type has a Grass Varieties array.
Each element in this array describe a [[Foliage]] type that can be spawned.
Each element has a Grass Mesh that is the [[Static Mesh Asset]] that should be rendered where this particular foliage has been spawned.
Set Grass Density and Scale X, Y, Z to get the wanted amount of grass.
Grass Density of about 100 is reasonable for small-ish grass meshes.

In the [[Landscape Material]], on the Grass node, in the Grass Types array in the Details panel, select a Landscape Grass Type for Grass Type.

The location of the Landscape Grass Type instances is controlled with [[Landscape Painting]].
The Grass node is told about the Landscape Grass Type to Landscape Layer relationship with a Landscape Layer Sample node.
The Landscape Layer Sample node has a Parameter Name property that should be set to the name of the layer that should spawn instances set on the Grass node.
Connect the output pin of the Sample node to the input pin on the Grass node.

