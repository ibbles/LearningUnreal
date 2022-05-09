These are some of the properties that describe a light source.
Not all [[Light Sources]] have all of these properties.

- Intensity: The strength of the light. I think different light source types have different units.
- Attenuation Radius: 
- Max Draw Distance: The distance beyond which the light will not be included. I assume this is distance between the light and the camera and does not have anything to do with the position of the illuminated object, that's Attenuation Radius.
- Max Distance Fade Range: The thickness of the hull inside the Max Draw Distance in which the light's intensity will decrease towards zero as the distance from the camera to the light increases towards Max Draw Distance.
- Use Inverse Squared Falloff.
  - On is the more physically correct mode. Off is better for fill lights. The main difference is that with Use Inverse Squared Falloff disabled one can control the Light Falloff Exponent which makes it possible to control the shape of the light attenuation, making it possible to have light sources with an Attenuation Radius that closely follows the point where the falloff makes the light contribution invisible. With User Inverse Squared Falloff enabled, the default, the radius will often be larger than the visible contribution from the light, especially for strong light sources with long attenuation radius.

