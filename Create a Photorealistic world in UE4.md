Summary notes of [Create a Phtotorealistic world in UE4 @ youtube.com by Quixel](https://www.youtube.com/watch?v=ATX7kmET4zE).

The video is from 2017, which was a few years ago.
Not sure how much has changed since.

The bits of information here should be moved either to dedicated notes, or merged into the other Landscape and Megascans notes.

# Textures
Textures:
- Albedo  
The color of the object without any light/shadow information.
- Cavity  
Small-scale shadows.
Grayscale with dark areas where there are cavities in the surface.
Set Compression Settings to `Masks`, since grayscale.
- Normal
Boost detail of low-poly mesh.
Set Compression Settings to `Normalmap` and enable `Flip Green Channel`.
- Roughness
Grayscale with bright areas where the surface is rough, dark areas where it is shiny.
Set Compression Settings to `Masks`, since grayscale.
- Opacity  
Control which areas a shown and which are not. 0/1 bit mask.
- Translucency
Measure the "thickness" of translucent materials, often foliage. It is a color texture, not just a depth map. Called subsurface color in Unreal Engine.

# Master Materials
There are two types of materials:
- Opaque  
Used for things such as rocks.
- Foliage  
Used for foliage.

Create a [[Material Instance]] of the appropriate master material for each mesh you import.
Set the correct textures for each texture parameter.

## Opaque
Sample albedo texture, multiply by a brightness scalar parameter.
Sample cavity texture, using [[Sampler Type]] `Masks` , raise to a scalar parameter power, multiply by albedo into Base Color.

Set specular to one of the albedo channels multiplied by a scalar parameter.
This is to avoid the white sheen that the default specular of 0.5 brings.

Sample roughness texture, using [[Sampler Type]] `Masks`, multiply by scalar parameter, plug into the Roughness input pin on the material output node.

Sample normal texture, using [[Sampler Type]] `Normal`, scale red and green by a scalar parameter, append all channels back together.

![](./Images/Create a photorealistic workd in UE4 opque master material.jpg)

A trick is to add a Dither Temporal AA node, multiply it by a Scalar Parameter, and pass to Pixel Depth Offset.
This will make the mesh blend into its surroundings.
I think shadows must be turned off for this to work, mentioned in another video. (Source/link?)

## Foliage
On the material output node:
- Set [[Blend Mode]] to Masked.  
To enable the Opacity Mask input pin on the material output node.
- Set [[Shading Model]] to Two Sided Foliage.   
To enable the Subsurface Color input pin on the material output node.
- Enable the Two Sided flag.

Parts of it is identical to the opaque master material.
Does not have a cavity map.
Does have opacity and translucency / subsurface color maps.

Sample the opacity texture, multiply by a scalar parameter, and wire to Opacity Mask.

Sample the translucency texture , multiply by a scalar parameter, and wire to Subsurface Color.

![](./Images/Create a photorealistic workd in UE4 foliage master material.jpg)

Recommends increasing the Normal Strength scalar parameter a bit, may 2.0.

# Level Of Detail
Each Mesh asset can contain multiple meshes of different complexity.
The engine will switch between them based on distance or screen size.
We add new LOD levels from the Static Mesh Editor.
Under LOD Settings > LOD Import.
LOD level 0 is the highest quality mesh, increasing LOD levels should decrease the quality.
Each LOD level can have a separate Material.
Some Megascans assets come with different textures for different LOD levels.
So we need a different [[Material Instance]] for each as well.


# Placing Rocks
Rocks are placed as Static Mesh Actors.
(
Would Instanced Static Mesh be better?
Should one do something with HLOD?
)

To fill the environment with some rocks, simply duplicate, scale, move, and rotate a small number of rock assets.

# Lighting and Atmosphere
## Directional Light
Sunlight is provided by a [[Directional Light]].
Setting a light's Mobility to Movable makes the light fully dynamic.
This is useful when building the level.
May want to switch to Static or Stationary later, for performance.

## Sky Light
Provides ambient light.
Fake bounce lighting where the Directional Light doesn't hit.
Disabling the Directional Light may help while configuring the Sky Light.
Finding a good balance between Directional Light and Sky Ligth is difficult.

Turning off Lower Hemisphere Is Solid Color brightens indirect lighting and makes it possible to set its color.
Make the color match the overall color of the scene.
When it is on one can select a Tint color instead.
Not sure what the difference is.
Used to avoid black shadows.

## Exponential Height Fog
Set Start Distance so only distance part of the [[Landscape]] is affected.
Kills realism if you go too crazy with this.
Experiment with Start Distance, Height Falloff and Density parameters.

## Post Process Volume
Enable Unbounded to affect the entire scene.

### Ambient Occlusion
A form of shadowing.
Increase Fade Out Distance to cover the entire scene.
Tweak Intensity, Radius, and Power until it looks good.
Radius is the distance one object can occlude another.

### Auto Exposure
Tweaking the scene with Auto Expose is difficult because moving the camera changes the lighting.
Disable it by setting Min and Max Brightness both to 1.0.
Tweak Exposure Bias until it looks good.

### Depth of Field
Emulates focus of a camera.
Difficult to hard-code a good value for Focal Distance.
Usually off while working on the scene.
Useful for fancy screenshots.

### Foliage
Can disable Paint > Filters > Static Meshes to paint foliage only the the [[Landscape]].
Useful when adding grass or rocks around boulders.



[Create a Phtotorealistic world in UE4 @ youtube.com by Quixel](https://www.youtube.com/watch?v=ATX7kmET4zE)