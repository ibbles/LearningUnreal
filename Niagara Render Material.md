A Niagara Render Material is a regular [[Material]] that has [[Material Editor]] > [[Details Panel]] > Usage > Used With Niagara (Sprites|Ribbons|Mesh Particles) and optionally makes use of particle input nodes.

The [[Niagara Sprite Renderer]] renders particles as quads.
To get a quad in  the Preview [[Viewport]] select the plane primitive in the bottom-right corner of the Preview [[Viewport]].

Particle input nodes read data from [[Niagara Attribute]]s.
You can find the particle input nodes under Particles when right-clicking in the [[Material]] [[Node Graph Editor]].


- Particle Color
	- Gives RBGA color. It is common to connect the RGB output pin to the Base Color or Emissive Color input pin on the [[Material Output Node]]. Which depend on if Shading Model is Default Lit or Unlit. The A output pin is often connected to the Opacity input pin on the [[Material Output Node]].
- Particle Position WS: World space.
- Particle Radius
- Particle Relative Time
- Particle Direction
- Particle Speed
- Particle Macro UV
- Dynamic Parameter: See _Dynamic Parameter_ below.

As a starting point, you can double-click [[Niagara Emitter]] > any render Module > Selection panel > Material to open that [[Material]] in the [[Material Editor]].
For example, a Sprite Renderer by default uses the Default Sprite Material.
This will shown you some examples of what can be done.

A Niagara Render Material can use [[Material Output Node]] > Details panel > Material > Shading Model set to Unlit, Default Lit.
With Unlit a bunch of input pins on the [[Material Output Node]] is disabled,
color is set on the Emissive Color pin.

A Niagara Render Material can use [[Material Output Node]] > Details panel > Material > Blend Mode set to Additive.
This makes densely packed particles brighter together.

A Niagara Render Material can use [[Material Output Node]] > Details panel > Material > Blend Mode set to Translucent.
This makes it possible to have transparent particles without getting the brightness associated with Additive Blend Mode.


# Animated Texture

We can put an animated  texture on our sprite as follows:

TODO Image here.

```
                                          Tex Coord \
                                                      --- + Texture Sample
Cloud --- -0.5 --- *2 --- * (Time --- Sine --- Abs) /
noise
sample
```

# Dynamic Parameter

An input (red) node that that has a Parameter Index input pin and four Param float output pins.
Also has RGB and RGBA output pins, which are combinations of the Param outputs.
The index can be in the range 0..3, so four possible values.
Four indices with four values each gives sixteen parameters in total.

We can set the names of the output pins from the Dynamic Parameter node's [[Details Panel]], under Parameter Names.


The parameters are set in the particle system.
With a Dynamic Material Parameters [[Niagara Module]].
Added for example to the Particle Update phase.
The Selection panel for the Dynamic Material Parameters node contains four sections with four parameters each,
matching the four-by-four parameters available on the Dynamic Parameter node in the [[Niagara Render Material]].
Each section has a Write Parameter checkbox that must be enabled for this parameter group to be written.
The Dynamic Material Parameter Module will look at the [[Material]] set on the Render Module and read the parameter names from the Dynamic Parameter node in it.
I don't know what it does if there are multiple Render Modules with different [[Material]]s,
or if the [[Material]] contains Dynamic Parameter nodes with conflicting names.

The parameters on the Dynamic Material Parameters [[Niagara Module]] supports Dynamic Input, meaning we can enter expressions that read other parameters or [[Niagara Attribute]]s.
For example, we can create Float From Curve and user PARTICLES Normalized Age as the Curve Index.

# Choosing Material On A Render Module

The [[Material]] to use for a particular [[Niagara Renderer]] [[Niagara Module]] can either be set in [[Niagara Editor]] > Selection panel directly or using a binding.
To use a binding create a user [[Niagara Parameter]] of type [[Material Interface]] and set render module > Selection panel > Material User Param Binding to that parameter.

# References

- [_Begin Play | Niagara_ by Epic Online Learning, Arran Langmead @ dev.epicgames.com/tutorials 2023 UE5.0](https://dev.epicgames.com/community/learning/tutorials/j9YO/unreal-engine-begin-play-niagara)
 - [_Intro To Niagara_ by Epic Online Learning, James Hill @ dev.epicgames.com/tutorials 2023 UE5.2](https://dev.epicgames.com/community/learning/tutorials/8B1P/unreal-engine-intro-to-niagara)

