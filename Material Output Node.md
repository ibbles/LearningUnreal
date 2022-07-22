The Material Output Node is the most central part of a [[Material]].
It is the main contributor to what objects rendered with that [[Material]] will look like.
It is the sole [[Execution Nodes|Execution Node]] in a [[Material Graph]].
When a [[Material]] is evaluated, all the input pins on the Material Output Node are evaluated by walking their wires backwards until their beginnings and then accumulating the result back to the input pin.
That each pin does is explained in [[Material]].
