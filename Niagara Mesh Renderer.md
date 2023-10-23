Mesh Renderer is a type of [[Niagara Module]] that goes into the Render group of a [[Niagara Emitter]].
It is used to render a [[Static Mesh]] for each particle.

If the particles are rendered "ghostly" or smeary / smeared out with long trails then try setting Mesh Renderer > Motion Blur >> Motion Vector Setting to Precise.
I don't know what  this actually does.
I don't know if there is a way to directly control the motion vector.
Setting `PARTICLES.Velocity` doesn't seem to affect this.