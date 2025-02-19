Controls the usage scenario for a [[Material]].
- Surface.
	- The default.
- Deferred Decal.
- Light Function.
- Post Process Blendable
- User Interface
- Volume

Set at [[Material Editor]] > [[Details Panel]] > Material > Material Domain.

# Surface


# Deferred Decal

A [[Material]] designed to be used with a [[Decal]] [[Actor]].
A [[Decal]] is a [[Texture]]-like object that is projected onto other objects in the scene.
An extra layer on top of the regular rendering of that object.
When the Material Domain is set to Deferred Decal in the Details panel a new property appears: Decal Blend Mode.

Selecting Deferred Decal removes some of the [[Material Output Node]] input pins, such as World Displacement and Pixel Depth Offset.
Since the position of the pixel is controlled by the object the decal is projected on it is that [[Material]] that controls the Pixel Depth Offset.


# Light Function

A [[Material]] used with a [[Light Source]], projecting the [[Material]] where the [[Light Source]] shines.
It allows us to shape the light intensity, controller where it is strong and where it is weak.
Simulating something, such as a cover, obstructing or refracting the light.
A Light Function [[Material]] on has the Emissive Color input pin on the [[Material Output Node]].
The Emissive Color is treated as intensity only, i.e. grayscale.

Can be used for water caustics and rippling of water, cloud shadows, fire flickering.


# Post Process Blendable

Alters the way the screen is rendered.
Can read a special input node named Scene Texture,
which can be set to sample from the [[GBuffer|GBuffers]], for example Post Process Input.
Post Process Input is the final output of the renderer, what the end-user would see if no post-processing was enabled.
There are multiple Post Process Input selections, so you can read the output of another Post Process step.
Post Process Input 0 is before any post-processing has happened.
I don't know how to manage the selection of these in a sane way to get a global view of the post-process ordering.
What if different scenes want a particular Post Process Blendable [[Material]] to read from different Post Process Input stages?
The [[Material Output Node]] only has a single input pin: Emissive Color.
For each output pixel the responsibility of a Post Process Blendable is to generate the final color by reading and computing on the Scene Texture data.

Used with a [[Post Process Volume]].
Register at [[Post Process Volume]] > [[Details Panel]] > Rendering Features > Post Process Materials.

Used for fog, water effects, heat effects, rain, object outlines, cell shading or other stylizing, ...


# User Interface

Used for user interface elements such as a button.

Removes most of the [[Material Output Node]] input pins.
Base Color is replaced by Final Color.
Get a Screen Position input pin.

See also [[User Interface Material]].

# Volume

Produces a volumetric effect.
Often used with particle systems.
Renames the Base Color [[Material Output Node]] input pin to Albedo, adds Extinction, and disables all other input pins.


# References

- [_Materials Master Learning_ > _Material Domain_ by Epic Games @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/7M8/material-domain)

