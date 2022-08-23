Add [[Post Processing]] to an area of the map, or an entire level by enabling Infinite Extent (Unbounded).
An Actor that define a volume in a level in which a [[Post Processing]] should be applied.
The volume can either be bounded, i.e. only apply within a cube, or unbounded, also called Infinite Extent, i.e., applied everywhere.
Make the Post Process Volume infinitely large by enabling Details panel > Post Process Volume Settings > Infinite Extend (Unbound).
It is the position of the camera that determine of a Post Process Volume is active or not, not the position of the thing being rendered.
One cannot see the effect when looking into a Post Process Volume, but one can see it when looking out.
Controls the exposure of the scene, camera settings, [[Lumen]] quality settings, [[Ray Tracing]] settings, color grading, and more.

A Post Process Volume is an override mechanic.
For it to do anything a setting in the Details must first be enabled, and then that setting in the Post Process Volume overrides that of the [[Project Settings]] or a lower-priority Post Process Volume.

# Inter-Post Process Volume Priority

I think we can have overlapping Post Process Volumes and set a priority to control which on wins.


# Auto Exposure

Disable Auto Exposure with the following settings in Details panel > Exposure
- Set Metering Mode to Manual.
- Disable Apply Physical Camera Expose.
- Tweak the Exposure Compensation slider.

See also [[Brightness Auto Exposure]].


# Color Grading

Color grading is used to modify the look and feel of the scene.
Editing within the Global category will affect the entire spectrum.
The other options are Shadows, Midtones, and Highlights.

## Saturation

Can be used to remove color from the scene.
Reduce the RGB Y component.
Not sure what that means.

## Gamma

Controls the gamma curve.
Reducing this value makes dark areas darker and bright areas brighter.
(
I think.
)
Moving the small circle within the larger color circle increases the amount of that color in the scene.

# Rendering Features

## Ambient Occlusion

Disable [[Ambient Occlusion]] by setting Intensity to 0.0.


# Lumen

There are [[Lumen]] related settings in multiple categories of the Post Process Volume Details panel.
Under Reflections you can set the Method to Lumen.
Under Lumen Reflections you can set Quality from 1 (or 0?) to 4.
Ray Lighting Mode controls what hardware traced reflection rays trace against.
This can be either Surface Cache or Hit Lighting.
(What follows is speculation.)
Surface Cache means that only things in the Lumen scene is shown in reflections.
The Lumen scene is a low-resolution version of the scene that is fast to trace against.
Hit Lighting means that the reflection rays are traced against the actual geometry.

# References
- [_Lighting in Unreal Engine 5 for Beginners_ - Post Process Volume by William Faucher @ youtube.com](https://youtu.be/fSbBsXbjxPo?t=1785)
- [_# UE5 Hints & Tips - Foliage and Trees_ - Saturation, by UnrealityBites @ youtube.com](https://youtu.be/dxofebT02VI?t=1476)


