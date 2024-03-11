The Material Output Node is the most central part of a [[Material]].
It is the main contributor to what objects rendered with that [[Material]] will look like.
It is the sole [[Execution Node]] in a [[Material Graph]].
When a [[Material]] is evaluated, all the input pins on the Material Output Node are evaluated by walking their wires backwards until their beginnings and then accumulating the result back to the input pin.
That each pin does is explained in [[Material]].

Only a subset of the input pins are active, the others are grayed out.
Which pins are active and which are grayed out depend on how the Material Domain, Blend Mode, and Shading Model has been set in the Material Output Node's [[Details Panel]].

See also [[Shader]].