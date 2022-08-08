A stand-alone application shipped with Unreal Engine that builds baked lightmaps for [[Static Lighting]] and [[Global Illumination]].
Only works for [[Light Source]] with static or stationary [[Mobility]] and [[Actor|Actors]] with static [[Mobility]].
Gives better performance than dynamic [[Global Illumination]] techniques such as [[Light Propagation Volume]]

The Lightmass tool is a stand-alone application that is run as part of the project packaging, or from the Build menu, and results in [[Lightmap|Lightmaps]] used during rendering of static objects.

Lightmass can work in a distributed mode where multiple machines cooperate to build lighting for a level.

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

Should be added to openings in interior scenes, i.e doors and windows.
This will help Lightmass prioritize areas where light is likely to enter the interior from the outside sky.
It tells Lightmass to try and find ways to bounce light through that portal.
Appear as a volume that should be placed and scaled to match the opening.
Can reduce the light baking time since the more important areas are prioritized.


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
