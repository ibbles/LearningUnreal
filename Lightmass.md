An implementation of baked lightmaps for [[Global Illumination]].
Only works for [[Light Sources]] with static or stationary [[Mobility]] and [[Actor|Actors]] with static [[Mobility]].
Gives better performance than dynamic [[Global Illumination]] techniques such as [[Light Propagation Volume]]

The Lightmass tool is a stand-alone application that is run as part of the project packaging, or from the Build menu, and results in [[Lightmap|Lightmaps]] used during rendering.


# Requirements

## Static Actors

Marking an [[Actor]] as static tells the engine that they won't move or be animated during runtime.
This means that they can receive [[Lightmap|Lightmaps]].
Set [[Actor]] > Details panel > Transform > [[Mobility]] to Static.


## Static or Stationary Light Sources

Set the [[Light Mobility]] of all [[Light Sources]] to static.
This means that the light from that light will be fully baked.
With stationary light sources some of the lighting information, such as [[Global Illumination]], will be baked, but not direct light and shadows.



## Non-overlapping UVs on meshes

The output of [[Lightmass]], the baked [[Lighting]] information, is stored in textures per object.
In order for this to work it is important that every point on the mesh map to a unique point on the [[Lightmap]] texture.
Otherwise we will get light artifacts and light hitting part of the mesh will light up another part as well.
[[Material]] textures are often overlapping so that multiple parts of an object that should look the same can look the same without having to duplicate texture data.
It is therefore possible to have multiple UV channels per object, one or more for material textures and one for lightmaps.
The lightmap UVs are normally the last one.
The available channels can be seen in the [[Static Mesh Editor]].

Unreal Editor can generate lightmap UVs.
Enable [[Static Mesh Editor]] > Details panel > Build Settings > Generate Lightmap UVs.
There are a few settings to control the generation in the same Details panel category.
Destination Lightmap Index control which UV channel the lightmap UVs will be stored to, make sure you don't overwrite the material texture UVs.
Min Lightmap Resolution should match [[Static Mesh Editor]] > General Settings > Light Map Resolution, for padding between triangles in the map to match.
Not sure what the difference between those two are, why we need two, during what circumstances they should be different, what which one should be larger when they are different.


# Building Lighting

To build lighting, i.e. to fill in the [[Lightmap|Lightmaps]] with data, we need to trigger the Lightmass tool.
This is done from
- Unreal Engine 4: Main Tool Bar > Build > Build Lighting Only.
- Unreal Engine 5: Main Menu Bar > Build > Build Lighting Only.

This may take some time.
When done the results will be visible in the [[Level Viewport]] and the Preview markers will be removed.


# Visualization




# References

- [_Introducing Global Illumination_, by Epic Games @ dev.epicgames.com, UE-4.20](https://dev.epicgames.com/community/learning/courses/yon/introducing-global-illumination/yo8/introduction-to-global-illumination)

