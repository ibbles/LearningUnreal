Global illumination, also called indirect lighting, is the effect that light can bound multiple times, and with each bounce it is affected by the object it bounces off of.
Light that bounces off of a red object turns red, and thus causes other objects it hits to be illuminated by red light.
You can say that every object hit by light becomes a light source.

Since Unreal Engine 5 the global illumination implementation is [[Lumen]].

# Controlling Global Illumination
The amount of indirect lighting is controlled in three different ways:
- Material base color.
- The Details panel > Intensity of the [[Light Sources|Light Source]].
- The Details panel > Indirect Lighting Intensity of the light source.


## Material Base Color

This should be your first avenue when tweaking the indirect lighting of a scene, since it has basics in physics.
The base color of a [[Material]] directly influences the indirect lighting bouncing off of it.
By increasing the base color we increase the amount of indirect lighting accordingly.
Base color is the reflectance of our object; the brighter the object, the more light it reflects onto the surrounding objects.
And conversely, reducing the base color of an object will reduce the amount of light it reflects onto the environment.

## Intensity Of The Light

The indirect lighting is a function of the incoming light, so increasing the intensity of the light source will also increase the intensity of the indirect lighting.


## Indirect Lighting Intensity Of The Light

Increasing this property will increase the amount of indirect lighting created by the edited light source.


# References

- [_Lighting in Unreal Engine 5 for Beginners_ Indirect Lighting by William Faucher @ youtube.com](https://youtu.be/fSbBsXbjxPo?t=1067)

