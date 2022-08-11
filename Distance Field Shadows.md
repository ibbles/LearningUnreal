[[Project Settings]] > Engine > Rendering > Lighting > Generate Mesh Distance Fields must be enabled for Distance Field Shadows to be available.

A distance field is an approximation of a mesh stored as a volumetric texture.
The texture is quite coarse, i.e. it has low resolution compared to the triangle data.
Which means it can miss fine details such as twigs and leaves in trees.
But it is often enough resolution for shadows, at least at a distance.

Using the distance field instead of the triangles for shadow generation improves performance a lot.

The distance fields generated can be visualized by enabling [[Level Viewport]] > Show > Visualize > Mesh Distance Fields.

Whether to generate distance field shadows or not is a per-light setting in
[[Light Source]] > Details panel > Distance Field Shadows > Ray Traced Distance Field Shadows.
The property is named Distance Field Shadows, without the Ray Traced part, in Unreal Engine 5.

Distance Field Shadows take over when we're past the Dynamic Shadow Distance for the [[Cascaded Shadow Maps]] for the light.
Without Distance Field Shadows the object would become shadow-less when past the [[Cascaded Shadow Maps]] distance.

Distance Field Shadows do not support animation since the distance field is generated at build time.
This means that the swaying of branches and the like won't be seen in the shadows.

Distance Field Shadows does not have the aliasing problem that [[Shadow Maps]] have.
That is, a Distance Field Shadow doesn't get blocky where there isn't enough shadow map resolution because there is not shadow map.

I do not know what the relationship is between Distance Field Shadows and [[Virtual Shadow Maps]].
It does not seem to do the [[Virtual Shadow Maps]] first and Distance Field Shadows switch when moving the camera away from the shadow-casting object.
It seems Distance Field Shadows completely replace [[Virtual Shadow Maps]] for the [[Light Source]] when enabled.

# References

 - [_Lighting Essential Concepts and Effects - Dynamic Lighting - Outdoor_, by Epic Games @ dev.epicgames.com, 2018](https://dev.epicgames.com/community/learning/courses/Xwp/lighting-essential-concepts-and-effects/V0M/dynamic-lighting-outdoor)

