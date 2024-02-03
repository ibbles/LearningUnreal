A Material Layer is a type of [[Material Function]].
A Material Layer operates on entire material attribute sets, i.e. Base Color, Roughness, Metallic, Normal, and so on.
This data type is called Material Attributes.
The return value, i.e. output pin, of a Material Layer is of type Material Attributes.
A Material Attributes value is created with the Set Material Attributes node.
The members of a Material Attributes value is accessed with the Break Material Attributes node.
Build your [[Material Graph]] as you normally would, pass all the computed values to a Set Material Attributes node, and connect that to the output node.
This allows us to put the entire Material definition in a Material Layer.

A Material Layer can have [[Material Parameter|Material Parameters]], just like any other [[Material Function]].

The actual [[Material]] is responsible for evaluating Material Layers, blending the Material Attributes, and passing the result to the [[Material Output Node]].
Material Attributes are blended with any of the many Material Layer Blend nodes, name-prefixed with `MatLayerBlend_`.
The output value is always of type Material Attributes.
It is common to use a mask texture to control which material should go where,
which is passed to the Alpha input pin on the blend nodes.
Different channels in the mask can be used to control the amount of the different Material Layers at each point on the object.
It is common to blend multiple Material Layers together in a sequence,
in each step adding one more into the mix with its own Alpha value.

To pass the result to the [[Material Output Node]] we need a way to pass a Material Attributes to it.
Tick [[Material Editor]] > [[Details Panel]] > Material > Use Material Attributes,
which causes all input pins on the [[Material Output Node]] to collapse into a single Materials Attributes pin.

It is common to have one Material Layer for every [[Master Material]] in the project.

You can create a Material Function that operates on a Material Attribute by creating a [[Material Parameter]] of that type.
To access the individual Material Attributes members use the Break Material Attributes node.
This makes it possible to take a Material Attributes, tweak some properties of it, and then recombine again.

A Material Layer is an [[Asset]] created with [[Content Browser]] > right-click > Material & Textures > Material Layer.
They have a light-blue color bar.
Material Layer nodes have a blue title background in the [[Material Graph]].
By splitting the functionality of a [[Material]] over multiple [[Asset|Assets]] we make it easier for multiple people to work together.

# References

- [_Materials Master Learning_ > _Material Layers_ by Epic Games @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/DMX/material-layers)
