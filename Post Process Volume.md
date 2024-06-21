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
Color correction.
It allows for transformation of color spaces from HDR to LDR.
Unreal Engine works in HDR internally, to get more colors to work with, and maps those colors to LDR for non-HDR displays.
The Filmic Tonemapper uses Academy Color Encoding System, ACES.
(
I don't know what the Filmic Tonemapper is.
Is it all the settings in the Color Grading category of the Post Process Volume?
)

To help with color grading Unreal Engine includes a [[Static Mesh]] named `SM_ColorCalibrator`.
Make sure [[Content Browser]] > Settings > Show Engine Content is enabled to find it.


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

## Global, Shadows, Midtones, Highlights

A collection of color tweaking controls that all operate in the same way but have different effects on the image.

Can be in either RGB mode or HSV mode.

R, G, B (or H, S, V) sliders let us control the color channels individually.
Hold Shift while dragging a slider to make finer edits.
This moves a circle within the color wheel.
We can also move the circle and the sliders will update accordingly.
Under the color wheel is the Intensity slider.
It controls all the RGB (or HSV) values together, while maintaining relativity set with the individual sliders.
Think of the intensity slider as a multiplier on the RGB (or HSV) values.

There is a forth slider, Y, that controls luminance.
Increasing it brightens the saturation.

### Saturation

Can be used to remove color from the scene.
Reduce the RGB Y component.
Not sure what that means.

### Contrast

Add color to the highlights and shadows.

### Gamma

Controls the gamma curve.
Controls the midtones.
Reducing this value makes dark areas darker and bright areas brighter.
(
I think.
)
Moving the small circle within the larger color circle increases the amount of that color in the scene.

### Gain

Controls the highlights.

### Offset

Controls the shadows.

## Misc

### LUT

Color Grading LUT.
LUT is short for Look Up Table.
A chart that tells the renderer how to map the colors in our scene.
How color data should be read.
(
This means absolutely nothing to me.
)

Think of them as functions, implemented as a lookup table, that takes a color as input and returns another color as output.
The input is the pixels of the rendered image and the output is the pixels we see on screen.

There are different levels of LUTs.
A 1D LUT has a scalar input and a scalar output.
It operates on one channel at the time.
A 3D LUT takes a color input and produces a color output.
It operates on all three channels.

#### 1D LUT

A 1D LUT adjusts brightness, contrast, and black and white levels.
With a single 1D LUT the red, green, and blue channels are adjusted together, in the same way.
We can have three 1D LUTs, one each for the red, green, and blue channels.
This approach cannot mix colors, or have the intensity of one color affect the intensity of another.
These kind of LUTs clamp the output to 8-bit colors.
Means color space conversions will be lossy and HDR output is not possible.

The steps to create three 1D LUTs is to take a screenshot of the scene, open that in an image editing software, add adjustment layers to get the look you want, apply the same adjustments to a RGB table template image, import the adjusted template image to a Post Process Volume.

The basic pipeline is that we take a screenshot of the scene and open in the image editing software.
Then we use the adjustment layers available in the image editing software to produce the look we are after.
The same filters are also applied to an RGB table image template, altering the colors of that template.
This altered template is the definition of our LUT.
Export the template as a PNG image.
Import the PNG image into Unreal Engine as a [[Texture]].
Open it in the [[Texture Editor]] and set [[Details Panel]] > Level Of Detail > Mip Gen Settings to No Mipmaps.
Set the sibling property Texture Group to Color Lookup Table.

Create a Post Process Volume.
Enable Post Process Volume > [[Details Panel]] > Color Grading > Misc > Color Grading LUT.
Set Color Grading LUT to the imported [[Texture]].
The colors in the [[Level Viewport]] will now match those we created in the image editing software.

#### 3D LUT

A 3D LUT can perform more complicated color operations.
The color channels can interact with each other.
Can change one color to another.
Have a larger amount of color information.
(
What does this mean?
)

A 3D LUT is a color-to-color function.
We give it a color and can get any color in return.
It is implemented as a 3D, or Volume, [[Texture]] where the three channels of the input color form the coordinate within that texture.
That is, each input color owns a specific texel within the [[Volume Texture]] and that texel is the color that should be used displayed instead of the input color.

3D LUT files can have the `.exr` file extension.
When imported into Unreal Editor they become a [[Texture]] [[Asset]].
Convert the [[Texture]] to a [[Volume Texture]] with right-click > Create Volume Texture.
You need to know what the size of the [[Volume Texture]] should be.
Set this in [[Texture Editor]] > [[Details Panel]] > Source 2D > Tile Size X and Tile Size Y.
Set Compression > Compression Settings to HDR (RGB, no sRGB).

Create a [[Post Process Material]] with [[Volume Texture]] > right-click > Create Material.
This will create a new [[Material]] containing a [[Texture Sample]] node reading from the [[Volume Texture]].
Sett [[Material]] > [[Details Panel]] > Material > Material Domain to Post Process.
Connect the RGB output of the [[Texture Sample]] node to the Emissive Color input pin on the [[Material Output Node]]. 

Create a Scene Texture node and set [[Details Panel]] > Scene Texture ID to Post Process Input 0.
We don't need the alpha channel so pass the Color output to a Component Mask with the R, G, and B channels enabled and the A channel disabled.
Convert the RGB values to UVW coordinates for the [[Volume Texture]] [[Texture Sample]] node by multiplying them by `(size - 1) / size` and adding `0.5 / size.

Add the [[Post Process Material]] to Post Process Volume > [[Details Panel]] > Rendering Features > Post Process Materials.


# Film

The Slope parameter controls the steepness between the Toe and the Shoulder.
This affects the contrast of the image.
A low Slope makes the image washed out.

Toe and Shoulder set the chop-off values.
Toe controls the black levels and Shoulder controls the white levels.
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

## Motion Blur



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



