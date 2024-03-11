These are some of the properties that describe a light source.
Not all [[Light Source|Light Sources]] have all of these properties.

- [[Light Mobility]]:
- Intensity: The strength of the light, how bright it is. I think different light source types have different units.
- Intensity Units: The unit scale used for the light source. The outcome is the same, it is just a conversion factor. No impact on performance.
	- Unitless: No idea.
	- Lux: Used for [[Directional Light]]. Defined as the amount of light per unit area, wavelength weighted by the human eye sensitivity to that wavelength.  A value that by default goes from 0 to 10.
	- Candela, cd: Used for [[Point Light]] and [[Directional Light]]. Defined as the amount of light emitted in a single direction from a light source, wavelength weighted by the human eye sensitivity to that wavelength. A value that by default goes from 0 to 160.
	- Lumens, lm: The amount of light emitted per steradian. A steradian is like an area for angles, like a cone with the tip at the light source. If one candela is emitted in every direction within a one-steradian arc from a spot light, then the spot light emits one lumen in total light.
- Color: The color of the light.
- Color Temperature: An alternative to Color. Makes the light either blue or yellow.
	- I'm not sure how Color and Color Temperature interacts. They are not either-or.
- Source Radius: The size of the light emitting volume. Increase to get soft shadows. For a Directional Light the Source Radius is instead Source Angle. The Light must have Distance Field Shadows enabled for this setting to have any effect, or the Project Settings setting Shadow Map Method must be set to Virtual Shadow Maps.
- Attenuation Radius: Bound on the light's reach. Objects further away than this from the light won't be illuminated.
	- This is very important for performance. Keep it as small as possible.
- Max Draw Distance: The distance beyond which the light will not be included. I assume this is distance between the light and the camera and does not have anything to do with the position of the illuminated object, that's Attenuation Radius.
- Max Distance Fade Range: The thickness of the hull inside the Max Draw Distance in which the light's intensity will decrease towards zero as the distance from the camera to the light increases towards Max Draw Distance.
- Use Inverse Squared Falloff.
  - On is the more physically correct mode. Off is better for fill lights. The main difference is that with Use Inverse Squared Falloff disabled one can control the Light Falloff Exponent which makes it possible to control the shape of the light attenuation, making it possible to have light sources with an Attenuation Radius that closely follows the point where the falloff makes the light contribution invisible. With User Inverse Squared Falloff enabled, the default, the radius will often be larger than the visible contribution from the light, especially for strong light sources with long attenuation radius.
- Affects World: Exactly the same thing as Details panel > Rendering > Visible. Disabling this checkbox disables the light completely.
- Cast Shadows: Whether or not this light can cast [[Shadows]] of any kind. Must also enable some combination of Cast Static Shadows and Cast Dynamic Shadows.
- Shadow Resolution Scale: Set to less than one to reduce the shadow quality for this light. Set to zero to disable shadows completely.
- Indirect Lighting Intensity: How much the light from this light source bounces around the scene.
	- It is often better to control this with the Base Color of the [[Material|Materials]] of the objects in the scene, in particular large objects such as floors and walls.
- Volumetric Scattering Intensity: Control the intensity of [[God Rays Light Shafts]] from this light.

# Lighting Channels

A [[Light Source]] can be assigned to one or more lighting channels.
The same can be done for [[Mesh]]es.
A [[Mesh]] will only be lit by a particular [[Light Source]] if they share at least one lighting channel.
In this way we can control which objects in a scene are lighted by which light sources.
This can be for artistic or performance purposes.

# References
- [_Lux_ > _Illuminance_ @ wikipedia.org](https://en.wikipedia.org/wiki/Lux#Illuminance)
- [_Candela_ @ wikipedia.org](https://en.wikipedia.org/wiki/Candela)
- [_Lumen_ @ wikipedia.org](https://en.wikipedia.org/wiki/Lumen_(unit))
- [_Lighting Essential Concepts and Effects_, Lighting Basics - by Epic Games @ dev.epicgames.com](https://dev.epicgames.com/community/learning/courses/Xwp/lighting-essential-concepts-and-effects/W0K/lighting-basics)
