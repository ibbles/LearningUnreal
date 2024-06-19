A Post Process Volume overrides the [[Post Processing]] settings in an area of the map,
or an entire level by enabling Infinite Extent (Unbounded).
The volume can either be bounded, i.e. only apply within a cube, or unbounded, also called Infinite Extent, i.e., applied everywhere.
Make the Post Process Volume infinitely large by enabling [[Details Panel]] > Post Process Volume Settings > Infinite Extend (Unbound).

It is the position of the camera that determine if a Post Process Volume is active or not, not the position of the thing being rendered.
One cannot see the effect when looking into a Post Process Volume, but one can see it when looking out.

Create a Post Process Volume either by adding a Post Process Volume Actor or by adding a Post Process Volume Component to an existing [[Actor]] or [[Blueprint Actor]].
Post Process Volumes are often given a name with the `PPV_` prefix.

A Post Process Volume is used to control the exposure of the scene, camera settings, [[Lumen]] quality settings, [[Ray Tracing]] settings, color grading, and more.

The settings for a Post Process Volume are set in the [[Details Panel]].
For a setting in the [[Details Panel]] to activate it must first be enabled, and then that setting in the Post Process Volume overrides that of the [[Project Settings]] or a lower-priority Post Process Volume.
Many of the values in the [[Details Panel]] have a slider.
We can often set values higher than the slider maximum or lower than the slider minimum by typing a value.

A Post Process Volume has a Blend Weight.
This controls the strength of the effect on the final image.
This can be used to fade the effect in and out.
The Blend Weight is a value between 0.0 and 1.0.
Not sure what happens if the value goes outside these bounds.


# Inter-Post Process Volume Priority

We can have overlapping Post Process Volumes and set [[Details Panel]] > Post Process Volume Settings > Priority to control which on wins.
Higher priority wins.
I don't know if this is applied for the Post Process Volume as a whole,
or per overridden setting.
For example, consider the following overlapping Post Process Volumes and [[Project Settings]]:
- Priority 9
	- White Balance > Temp: Not overridden.
- Priority 1
	- White Balance > Temp: Overridden to 7'000.
- [[Project Settings]]
	- White Balance > Temp: 6'500.

Option 1: The priority 9 Post Process wins so it is applied. It does not override White Balance > Temp so the [[Project Settings]] remain in effect at 6'500.
Option 2: The priority 9 Post Process gets to go first but it doesn't override White Balance > Temp so the next in line, the one with priority 1, is considered. It does override White Balance > Temp so it is set to 7'000.


# Post Process Component

We can add a Post Process Component to an Actor.
A Post Process Component does not have an extent of its own.
It must either be set to Infinite Extent or be attached to a Shape Component.

# Auto Exposure

Disable Auto Exposure with the following settings in Details panel > Exposure
- Set Metering Mode to Manual.
- Disable Apply Physical Camera Expose.
- Tweak the Exposure Compensation slider.

See also [[Brightness Auto Exposure]].


# Lens

## Bloom

This category of settings control how an illuminated or [[Emissive Material]] leak color into surrounding pixels.
It produces fringes of light from the brightest areas of the image.

Method should be set to Standard for real-time applications.
If you want high-quality bloom then set Method to Convolution.

Intensity control how far an emissive surface will leak light.
Too much and it fogs up the scene.

Threshold control how strong the reflected or emissive light must be before it starts leaking into surrounding pixels.
Increase this value to get more localized bloom around bright lights and specular highlights.

## Exposure

Metering Mode controls how exposure is controlled.
- Auto Exposure Histogram: Automatic exposure.
- Auto Exposure Basic: Automatic exposure.
- Manual: Manual exposure.

With Manual one way to control the exposure is with the Camera > Shutter Speed and Camera > Aperture (F-stop).

We can visualize the current ??? with [[Level Viewport]] > [[Show]] > Visualize > HDR (Eye Adaptation).
This will show a box at the bottom of the [[Level Viewport]].
A bar diagram show the intensities for various colors. (Maybe, I don't know.)
The white line (Curve or vertical bar?) is our current exposure.
A blue bar shows the exposure target.
The green zone is the range that the adaptation can work within.

See [[Brightness Auto Exposure]].

Min and Max Brightness control how far down and up the exposure can move.
The size of the green box in the HDR (Eye Adaptation) [[Show]] overlay.
Set these equal to each other to force a single exposure setting, i.e. disable auto exposure.

## Chromatic Aberration

Simulates a distorted lens.
Creates separate layer for the three color channels and shifts them by Intensity.


## Dirt Mask

## Camera

Shutter Speed controls how much light is being let into the camera.

Increasing Aperture (F-stop) increases the focus depth and decreases the exposure.

## Local Exposure

Contrast Reduction: Not sure. It did something.

## Lens Flares

Intensity controls how visible the lens flare is.

Tint controls what color the lens flare is.

Threshold control how bright a surface needs to be to produce a lens flare.

## Image Effects

Vignette Intensity causes a dark halo around the image.


## Depth Of Field


# Color Grading

Color grading is used to modify the look and feel of the scene.
Editing within the Global category will affect the entire spectrum.
The other options are Shadows, Midtones, and Highlights.

## Temperature

A quick way to change the mood of a scene.

The Temperature Type setting switches between White Balance and Color Temperature.
They are inverses of each other.
White Balance: Blues are a colder temperature.
Color Temperature: Blues are a higher temperature.

Setting Temperature Type to White Balance and increasing Temp causes to scene to become less blue and more orange.

Tint does something I don't understand.

## Global

## Shadows

## Midtones

## Highlights

## Misc

Color Grading LUT.
LUT is short for Look Up Table.
A chart that tells the renderer how to map the colors in our scene.
(
This means absolutely nothing to me.
)

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


# Film

The Slope parameter controls the steepness between the Toe and the Shoulder.

Toe and Shoulder set the chop-off values.
A long toe will make shades of gray appear as black.
A short toe allow us to see more shades of gray, but decreases contrast.
Shoulder is like toe, but for white values instead of black.
A long shoulder will make near-white areas turn solid white.



# Rendering Features

## Post Process Materials

A list of [[Material]]s that can alter the scene in custom application-specific ways.
[[Post Process Material]]

Each [[Material]] has a floating point value associated with it.
This value lets us control how strong the influence from that [[Post Process Material]] should be on the scene.

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
- [_UE5 Hints & Tips - Foliage and Trees_ - Saturation, by UnrealityBites @ youtube.com](https://youtu.be/dxofebT02VI?t=1476)
- [_Introducing Post Processing_ by Epic  Online Learning, Kevin Lyle @ dev.epicgames.com/courses 2023 UE5.0](https://dev.epicgames.com/community/learning/courses/pE2/unreal-engine-introducing-post-processing/mZ11/unreal-engine-introducing-post-processing-overview)



