A Mesh Renderer is a type of [[Niagara Module]] that goes into the Render group of a [[Niagara Emitter]].
It is used to render a [[Static Mesh]] for each particle.

# Selecting Static Mesh

By default the Mesh Renderer renders each particle as a set of coordinate axes.
Change to a different [[Static Mesh Asset]] edit Selection panel > Mesh Rendering > Meshes.
This is an array of [[Static Mesh Asset]] to use.
By default the [[Static Mesh]] at index 0 is used.
The [[Static Mesh]] to render is controlled per particle with the PARTICLES > Mesh Index [[Niagara Attribute]].
This can be set in a Particle Spawn or Particle Update [[Niagara Module]] to control which mesh a particular particle should use.
The PARTICLES > Mesh Index attribute can be set with a Set New Or Existing Parameter Directly [[Niagara Module]].
In the Selection panel, on the Set Parameters category, click the + button, and type MeshIndex into the search box that appears.
PARTICLES > Mesh Index will appear in the list.
You can, for example, use a Random Range Int [[Niagara Dynamic Parameter]] to randomly select a [[Static Mesh]] to use.
Set Minimum to 0 and Maximum to one less than the number of [[Static Mesh]]es in the renderer's Meshes array.
I don't know how to bind the Maximum parameter to the array's Last Index.

# Scaling Static Mesh

The scale of the [[Static Mesh]] is controlled with the PARTICLES > Scale.
(
Not sure on this one.
The Initialize Particle [[Niagara Module]] has Selection panel > Mesh Attributes > Mesh Scale Mode = Uniform > Mesh Uniform Scale.
It sounds like Mesh Scale is separate from the generic Scale.
)


# Smeary Particles Ghosting

If the particles are rendered "ghostly" or smeary / smeared out with long trails then try setting Mesh Renderer > Motion Blur > Motion Vector Setting to Precise.
Somewhere around Unreal Engine 5.1 the Motion Blur part of the path was removed, it can now be found at Mesh Renderer > Rendering > Motion Vector Setting.
I don't know what  this actually does.
I don't know if there is a way to directly control the motion vector.
Setting `PARTICLES.Velocity` doesn't seem to affect this.

See [[Niagara Render Material]].

# References

- [_Begin Play | Niagara_ by Epic Online Learning, Arran Langmead @ dev.epicgames.com/tutorials 2023 UE5.0](https://dev.epicgames.com/community/learning/tutorials/j9YO/unreal-engine-begin-play-niagara)
- [_Intro To Niagara_ by Epic Online Learning, James Hill @ dev.epicgames.com/tutorials 2023 UE5.2](https://dev.epicgames.com/community/learning/tutorials/8B1P/unreal-engine-intro-to-niagara)

