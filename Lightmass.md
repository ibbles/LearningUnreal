A stand-alone application shipped with Unreal Engine that builds baked lightmaps for [[Static Lighting]] and [[Global Illumination]].
Only works for [[Light Source]] with static or stationary [[Mobility]] and [[Actor|Actors]] with static [[Mobility]].
Gives better performance than dynamic [[Global Illumination]] techniques such as [[Light Propagation Volume]] or [[Lumen]].

The Lightmass tool is a stand-alone application that is run as part of the project packaging, or from the Build menu, and results in [[Lightmap|Lightmaps]] used during rendering of static objects.

Lightmass can work in a distributed mode where multiple machines cooperate to build lighting for a level.


# Linux

I've had some issues getting light building to work on Linux, around the 4.25 - 5.1 time frame.
One [suggestion](https://discord.com/channels/187217643009212416/375022233875382274/1076284173951705149) on the Unreal Slackers Discord channel is to enable the UDP Messaging plugin.
(
I believe that plugin is enabled by default, more testing needed.
)
There is also, in the same Discord thread, [mentions](https://discord.com/channels/187217643009212416/375022233875382274/1076166929116577882) of separate building the UnrealLightmass build target separate.



# Requirements

## Static Actors

Marking an [[Actor]] as static tells the engine that they won't move or be animated during runtime.
This means that they can receive [[Lightmap|Lightmaps]].
Set [[Actor]] > Details panel > Transform > [[Mobility]] to Static.


## Static or Stationary Light Sources

Set the [[Light Mobility]] of all [[Light Source]] to static.
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


## GPU Lightmass

GPU Lightmass is a GPU accelerated version of Lightmass.
This has some extra requirements:
- Ray Tracing capable GPU, i.e. an RTX card from NVIDIA. Not sure if AMD cards are supported.
- Ray Tracing enabled at [[Project Settings]] > Engine > Rendering > Hardware Ray Tracing > Support Hardware Ray Tracing.
- Virtual Texturing enabled at [[Project Settings]] > Engine Rendering > Virtual Textures > Enable Virtual Texture Support.
	- Also Enable Virtual Texture Lightmaps in the same category.
	- Virtual Texturing and Virtual Texture Lightmaps are optional. Not sure what the advantages and disadvantages are. One advantage is that it enables progressive rendering in the viewport.
- Enable the GPU Lightmass plugin.
- Temporarily disable all ray tracing effects in the [[Viewport]].
	- `r.RayTracing.ForceAllRayTracingEffects 0`
	- I think this is only while running the bake, once done I think we can enable ray tracing again.

There is supposed to be a GPU Lightmass entry in the Main Tool Bar > Build menu, but I don't see it.
Something is wrong.
See _Not Getting Indirect Lighting From Baked Lighting_ in [[Troubleshooting Errors]].

GPU Lightmap can run in a slow / interactive mode or a fast / offline mode.
This is controlled from the Realtime [[Viewport]] setting available in the top-left drop-down.
In the interactive mode intermediate results are displayed in the [[Viewport]],
both a preview of the result and a per-lightmap texture progress bar.
In the fast mode the is no live / realtime update of the viewport and the computation can therefore finish faster.
The interactive more require that [[Runtime Virtual Texture]] and Virtual Texture Lightmaps are enabled in the [[Project Settings]].


### GPU Lightmass Settings

The GPU Lightmass panel contains a number of settings.

- Global Illumination
	- GI Samples: The number of rays to trace per [[Lightmap]] texel (I think.). Controls the quality of the the global illumination. Higher value means higher quality but longer baking time. 2048 is high, and will likely take a long time to bake.
	- Stationary Light Shadow Samples: Increase to reduce artifacts in shadows. No idea what this actually means.
	- Use Irradiance Caching: Not sure. An alternative algorithm for indirect lighting? No idea when and for what reasons this should be enabled or disabled.
	- Use First Bounce Ray Guiding: Run a few extra samples, called trial samples, so the subsequent samples can be more smartly directed. If weights the rest of the samples closer to the brightest trial samples. Enabling this can help guide light to spots that turn out to dark with Use First Bounce Ray Guiding disabled. No idea what the compute cost for this feature is.
- Irradiance Caching.
	- Quality: A ray count, I guess. Keep it 25% of Global Illumination > GI Samples.
- First Bounce Ray Guiding
	- Trial Samples: The number of trial samples to take before starting the lighting calculations for real. Keep it at 25% of Global Illumination > GI Samples.

# Configuration

## Lightmass Importance Volume

A volume that tell Lightmass where to focus the calculations.
It is a type of [[Actor]].
There should always be a Lightmass Importance Volume in the level since it does more than what is described here.

Set Location and Scale and/or Brush Settings > X|Y|Z to cover your level, or the bits where you want the best quality lighting.
Where you expect the camera to be.
Don't have to be extremely precise.
Improves the quality in that area of the level.
Can reduce the light baking time since less computation need to be expended on other parts of the level.



## Lightmass Portal

Should be added to outdoor-to-indoor openings in interior scenes, e.g doors, windows, holes, and so on.
This will help Lightmass prioritize areas where light is likely to enter the interior from the outside sky.
It tells Lightmass to try and find ways to bounce light through that portal.
Especially important in buildings with very small / narrow windows, such as a medieval castle.
Appear as a volume that should be placed and scaled to match the opening.
Can reduce the light baking time since the more important areas are prioritized.

It may be that Lightmass Portals are only considered by [[Sky Light]] and possibly [[Directional Light]].

## Lighting Quality

Set from
- Unreal Engine 4: Main Tool Bar > Build > Lighting Quality.
- Unreal Engine 5: Main Menu Bar > Build > Lighting Quality.

Can be one of
- Production:
- High:
- Medium:
- Preview: Fast to build but low quality.

Higher levels produces more accurate lighting with fewer artifacts, but takes more time.
I don't know what is changed between each quality level.


## World Settings

There are a number of Lightmass related settings in > [[World Settings]] > Lightmass.

### Indirect Lighting Quality

Increasing this value increases indirect lighting quality but makes light bake times increase.
One suggestion is to set it to 5.0.
I have no idea what this means, what it changes, or how one can know what a good value is for a particular level and use-case.

### Num (Indirect|Sky) Lighting Bounces

The number of times light from a [[Light Source|Light Source]] can bounce.
Increase this if you have dark corners that you believe should receive more bounce/indirect light.
There are different settings for [[Sky Light]] and all other types of light.
Don't know why Sky Light needs a separate setting.
Is the bounce count typically higher or lower for the Sky Light compared to other types of light?
I assume higher.

### Static Light Level Scale

Decrease this value to bring out small contact shadows.
A suggestion is 0.5.
May produce more noise, so may need to increase Indirect Lighting Quality to compensate.


# Light Source Mesh

It is possible to turn a [[Mesh]] into a light source.
For example a plane, representing the panel, in front of a TV to make it appear as-if the TV is turned on.
Create a [[Material]] for the panel, add a [[Texture]] Sample node sampling a texture with the image that the TV should show, and connect to the [[Emissive Material|Emissive]] input pin of the [[Material Output Node]].
Set [[Material]] > Details panel > Material > Shading Model  to Unlit.
Apply the new [[Material]] to the panel plane.

Enable plane mesh [[Actor]] > Details panel > Lighting > Lightmass Settings > Use Emissive For Static Lighting.
Set Emissive Boost in the same category to control the intensity of the light.

# Building Lighting

To build lighting, i.e. to fill in the [[Lightmap|Lightmaps]] with data, we need to trigger the Lightmass tool.
This is done from
- Unreal Engine 4: Main Tool Bar > Build > Build Lighting Only.
- Unreal Engine 5: Main Menu Bar > Build > Build Lighting Only.

This may take some time.
When done the results will be visible in the [[Level Viewport]] and the Preview markers will be removed.


# Visualization

When working with Lightmass it can help to switch the [[View Mode]] to Lighting Only.
This is especially helpful when there are objects with an [[Emissive Material|Emissive Material]] and Use Emissive For Static Lighting enabled.
And to disable [[Level Viewport]] > Show > Lighting Features > Screen Space Ambient Occlusion.
And to disable [[Level Viewport]] > [[View Mode]] > Post Processing > Bloom.
And to set [[Level Viewport]] > [[View Mode]] > Exposure > Fixed At some value.
This makes it easier to see the contributions from [[Static Lighting]] only.



# References

- [_Introducing Global Illumination_, by Epic Games @ dev.epicgames.com, UE-4.20](https://dev.epicgames.com/community/learning/courses/yon/introducing-global-illumination/yo8/introduction-to-global-illumination)
- [_Lighting Essential Concepts and Effects_ - Baked Lighting - Lightmass, by Epic Games @ dev.epicgames.com, UE-4.27](https://dev.epicgames.com/community/learning/courses/Xwp/lighting-essential-concepts-and-effects/229/baked-lighting-lightmass)
- [_Bake Lighting FASTER with GPU Lightmass - Unreal Engine 4.26_ by William Faucher @ youtube.com](https://youtu.be/hq1WFFF6iD0)
- [_Lighting with GPU Lightmass | Tips & Tricks | Unreal Engine_ by Epic Games @ youtube.com](https://www.youtube.com/watch?v=RBY82TSLjFA)




