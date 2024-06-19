Post Processing comes either from a
- [[Post Process Volume]].
- Post Process Component. (Is this the same as a [[Post Process Volume]]?)
- Settings on a Camera.
- Project Settings.

Post Processing happens near the end of the [[Rendering Pipeline]].
[[Mesh]]es have already been rendered and [[Lighting]] applied.
Post Processing takes that rendered image, the [[GBuffer]], as input and modifies it.
We say that Post Processing is applied in screen space.

Post Processing changes the look of your scene.
Can add things  like lens flares and bloom effects, color grading and tone mapping.
Some of these effects are enabled by default and can be configured using a [[Post Process Volume]].
They are configured in [[Project Settings]] > Engine > Rendering
- Global Illumination
- Reflections
- Lumen
- Default Settings

There can only be one Post Process active at the time.
If there are multiple Post Process that want to be active at the same time then the one with the highest Priority is used.
A Post Process has a Blend Weight that control the strength of the effect.
The Blend Weight makes it possible get a combination of multiple Post Processes.
(
I think.
Or is is it only a way to reduce the strength of a single Post Process?
)
(
Don't know how Priority and Blend Weight work together.
Does it only blend Post Processes with the same Priority?
)

There are many settings in the Details panel for a Post Processing entity.
Many of them with check boxes to enable.
A checkbox being disabled does not mean that the effect is off.
It means that this Post Processing entity doesn't override the setting of a prior Post Processing entity.
The actual setting can be seen/set with console variables, such as `r.SSR.Quality`.
Setting that to 0 will turn Screen Space Reflections off regardless of what the Post Processing settings are.

# Blendables
Created in the [[Material Editor]].
Modifies the Post Processing settings.
Often applied as a small Post Process Volume inside a larger one.
Enabled by adding Materials to Post Process Volume > Rendering Features > Post Process Materials.

The Material should have the Material Domain set to Post Process.
This will enable the Emissive Color output node input pin only.
Gives access to the special node Scene Texture : Post Process Input 0.
This is the rendered frame as seen by the Post Processing.
Can also read Scene Depth, Absolute World Position, etc.
The color passed to Emissive Color is the color that is shown on screen.

Can be used to generate under water effects, stylized looks, depth fog effects, custom color correction, heat/cold effects.

# Bloom
Chose between standard and the more expensive convolution.
Convolution often not used by games.

Does multiple bloom types and then blends them together.
The types are listed in Post Process > Details panel > Bloom > Advanced.
The different types can have different sizes.
Each can have different color and intensity.

# Camera Effects
Kinda like built-in Blendables.
Grain, vignette, chromatic abberation, lens flares, dirt mask.

# Color Grading
## White Balance
## Global
### Saturation
Set a low value to reduce the amount of color in the scene.

# Depth Of Field
Three types
- Gaussian
	- Cheapest.
- Bokeh
	- High quality but expensive.
	- Superseded by Circle/Cinematic.
- Circle/Cinematic
	- High quality, decent performance.

# Exposure
Dynamically change the light sensitivity based on the amount of light hitting the camera.
Can set maximum and minimum limit to avoid large changes in how the scene looks.
Often set to 1.0..1.0 during level design.

# Screen Space Ambient Occlusion

# Screen Space Global Illumination
Approximate global illumination based on the rendered image.

# Screen Space Sub Surface Scattering
Render layered materials.
Often used for skin and leaves/vegetation.
Not sure how this relates to [[Material]] sub-surface scattering.


# Tone Mapper
Does color/tint/contrast correction.

Enable a sharpen filter by running `r.Tonemapper.Sharpen <amount>` in the console.
10 for the value is a lot, 0.5 is reasonable.


# References

- [_Introducing Post Processing_ by Epic  Online Learning, Kevin Lyle @ dev.epicgames.com/courses 2023 UE5.0](https://dev.epicgames.com/community/learning/courses/pE2/unreal-engine-introducing-post-processing/mZ11/unreal-engine-introducing-post-processing-overview)
