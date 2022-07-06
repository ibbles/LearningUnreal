These are some of the properties that describe a light source.
Not all [[Light Sources]] have all of these properties.

- [[Light Mobility]]:
- Intensity: The strength of the light. I think different light source types have different units.
  - Lux: Used for [[Directional Light]]. Defined as the amount of light per unit area, wavelength weighted by the human eye sensitivity to that wavelength.  A value that by default goes from 0 to 10.
  - Candela, cd: Used for [[Point Light]] and [[Directional Light]]. Defined as the amount of light emitted in a single direction from a light source, wavelength weighted by the human eye sensitivity to that wavelength. A value that by default goes from 0 to 160.
- Color: The color of the light.
- Color Temperature: An alternative to Color. Makes the light either blue or yellow.
- Source Radius: The size of the light emitting volume. Increase to get soft shadows. For a Directional Light the Source Radius is instead Source Angle. The Light must have Distance Field Shadows enabled for this setting to have any effect, or the Project Settings setting Shadow Map Method must be set to Virtual Shadow Maps.
- Attenuation Radius: Bound on the light's reach. Objects further away than this from the light won't be illuminated.
- Max Draw Distance: The distance beyond which the light will not be included. I assume this is distance between the light and the camera and does not have anything to do with the position of the illuminated object, that's Attenuation Radius.
- Max Distance Fade Range: The thickness of the hull inside the Max Draw Distance in which the light's intensity will decrease towards zero as the distance from the camera to the light increases towards Max Draw Distance.
- Use Inverse Squared Falloff.
  - On is the more physically correct mode. Off is better for fill lights. The main difference is that with Use Inverse Squared Falloff disabled one can control the Light Falloff Exponent which makes it possible to control the shape of the light attenuation, making it possible to have light sources with an Attenuation Radius that closely follows the point where the falloff makes the light contribution invisible. With User Inverse Squared Falloff enabled, the default, the radius will often be larger than the visible contribution from the light, especially for strong light sources with long attenuation radius.
- Indirect Lighting Intensity: How much the light from this light source bounces around the scene.


# References
- [_Lux_ > _Illuminance_ @ wikipedia.org](https://en.wikipedia.org/wiki/Lux#Illuminance)
- [_Candela_ @ wikipedia.org](https://en.wikipedia.org/wiki/Candela)

