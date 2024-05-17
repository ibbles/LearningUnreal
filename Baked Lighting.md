The **output** of lighting building is a set of [[Lightmap|lightmaps]].
The actual building is performed by an application called [[Lightmass]].
Launching the lighting building will run a Swarm Agent.
[[Lightmass]] is a distributed application with a server and potentially many agents.

**Input** to the light baking are static lights and optionally a [[Lightmass Importance Volume]].
The baking is done using a radiosity algorithm.

The **output** from the light baking is Volumetric Lightmaps and Volume Lighting Samples.
Volume Lighting Samples is a less accurate older system still maintained for lower-end hardware.
Volume Lighting Samples captures a single color per sample.
Volumetric Lightmaps captures lighting from multiple directions.

Another set of baked lighting quality are available in the [[World Settings]], under [[Lightmass]].

To bake lighting, click the Build button in the [[Main Tool Bar]].
The light baking can be done at different quality levels,
selected from the drop-down menu on the right side of the Build button > Lighting Quality.
Start from a lower quality level to get the basics in place,
and then move to higher and higher quality settings as you get closer and closer to the final lighting setup.

The time it takes to build baked lighting depend on
- The [[Lightmap]] resolution of all meshes that receive baked lighting.
  - (Is that all meshes? How can I turn it off for a mesh?)
- The number of static (and stationary?) lights.
- The number of meshes that receive baked lighting.
- The Radius and Source Radius of the lights.
  - Not sure what this is. There is a Source Radius in the Details panel of a [[Point Light]]. Is Radius the [[Light Source Properties|Attenuation Radius]]?

They can be visualized with Viewport > Show > Visualize > {Volume Lighting Samples, Volumetric Lightmap}.
Shows up as spheres aligned in a grid pattern in the world, in and around Lightmap Importance Volumes.

The Volume Lighting Samples are used when rendering dynamic meshes, such as characters.
The spheres/grid record the baked ambient/indirect lighting.
As the mesh moves through the world the renderer reads the lighting information from the nearest spheres/grid cells.


# References

- [_Becoming an Environment Artist in Unreal Engine_ > _Lighting_ by Epic Online Learning @ dev.epicgames.com/courses 2020 UE 4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/oE6/unreal-engine-lighting)
