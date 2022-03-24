The output of lighting building is a set of [[Lightmap|lightmaps]].
The actual building is performed by an application called [[Lightmass]].
Launching the lighting building will run a Swarm Agent.
Lightmass is a distributed application with a server and potentially many agents.

Input to the light baking is static lights and Lightmass Importance Volume.
The baking is done using a radiosity algorithm.

The output from the light baking is Volumetric Lightmaps and Volume Lighting Samples.
Volume Lighting Samples is a less accurate older system still maintained for lower-end hardware.
They can be visualized with Viewport > Show > Visualize > {Volume Lighting Samples, Volumetric Lightmap}.
Shows up as spheres aligned in a grid pattern in the world, in and around Lightmap Importance Volumes.

