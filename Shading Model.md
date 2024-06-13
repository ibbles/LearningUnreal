A property of a [[Material]].
Set in the [[Material Editor]] > [[Details Panel]] >  Material > Shading Model with the Material's output node selected.
Or, to override the parent material's shading model in a [[Material Instance]], at [[Material Instance Editor]] > General > Material Property Overrides > Shading Model.
A [[Material Instance]] can override the parent [[Material|Material's]] Shading Model from [[Details Panel]] > General > Material Property Overrides > Shading Model.
Selecting different shading models causes different input pins to be available on the [[Material Output Node]].
The available shading models are:
- Unlit
- Default Lit
- Subsurface
- Clear Coat
- Two Sided Foliage
- Hair
- Cloth
- Eye
- and a few more.

# Unlit

An unlit material has not light-related input pins on the [[Material Output Node]].
Base Color, Metallic, Specular, Roughness, and Normal are all grayed out.
The only color-related pin enabled is Emissive Color.
We also have access to the position-related pins World Position Offset and Pixel Depth Offset.

A shading model that is computationally very cheap.
If a [[Material]] can be unlit and still look OK then it probably should be unlit.
Example use-case:
- Lamp.


# Default Lit

The default shading model.
Takes incoming light into account.

# Subsurface

Similar to Default Lit.
Adds the Subsurface Color and Opacity input pins on the [[Material Output Node]].
Opacity, for subsurface, does not make the entire object itself transparent,
but controls how much of the subsurface color that is visible through the base color.
(
Or maybe not. It actually controls how much light is let through from a [[Light Source]] behind the object.
Not sure what the Subsurface Color input pin does.
)
Use e.g. a [[Texture]] to control which parts of the model should let a lot of light through and which parts should block a lot of light.

Might not work with static lights, not sure.

Example use-case.
- Human bodies, e.g. ears, faces.
- Wax candles.
- Can be used for vegetation, but is there is a shading model specifically designed for vegetation.


# Subsurface Profile

Similar to Subsurface in that it gives subsurface scattering and enables the Opacity [[Material Output Node]] input pin.
It does not enable the Subsurface Color input pin.
This is a cheaper variant of Subsurface, it doesn't do any actual subsurface scattering calculations.

In the [[Details Panel]] a property named Subsurface Profile shows up.
This is a reference to an [[Asset]], which can be created with [[Content Browser]]  > right-click > Materials & Textures > Subsurface Profile.
The Subsurface Profile asset contains:
- Scatter Radius: How much the light goes through the material, how much it penetrates and scatters within.
- Subsurface Color: 
- Falloff Color:

It is faking subsurface scattering by blurring the surface of the model.
The blurring happens in screen-space, as a post-process effect.
Set the Scatter Radius to something large, e.g. 512, to get some insight into what the algorithm is doing.
It duplicates the image that was rendered, offsets that by the Scatter Radius, and blends it back in. Multiple times.



# Clear Coat

Gives a second set of roughness and normal.
Used to create the illusion of a rough surface covered by a smoother transparent surface.
Such as a layer of clear coat over some other material or object.

Enables two [[Material Output Node]] and creates an extra free-floating node named Clear Coat Bottom Normal.
The Clear Coat Bottom Normal is not part of the [[Material Output Node]] because it can be disabled from the [[Project Settings]],
see below for instructions.
The Clear Coat Roughness and Clear Coat Bottom Normal control the properties of the ...
(
Not sure if the extra input pins is for the clear layer on top or the material underneath.
)

The second normal is onlyavailable if [[Project Settings]] > Engine > Rendering > Materials > Clear Coat Enable Second Normal is ticked.
This can be disabled to improve performance.

# Two Sided Foliage

Used for foliage.
Enabled Subsurface Color input pin on the [[Material Output Node]].
Set this to some color that should be visible when the leaf is illuminated from behind.
Can be read from a [[Texture]].



# Skin / Hair / Cloth Eye



# References

- [_Materials Master Learning_ > Shading Model by Epic Games @ dev.epicgames.com 2019 ](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/lVe/shading-model)
