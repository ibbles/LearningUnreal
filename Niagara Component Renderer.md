A [[Niagara Renderer]] that instantiates an [[Actor Component]] for each particle.
Set the Component type at Selection panel > Component Rendering > Component Type.

To create an entire [[Actor]], use the [[Child Actor Component]].
After setting Component Type to Child Actor Component, set Selection panel > Component Properties > Child Actor Component > Child Actor Class to the [[Actor]] type to spawn.

I don't know how writes to PARTICLES > Position interacts with position control done by the [[Actor]].
What if the [[Actor]] has Simulate Physics enabled?

