Related to [[Dynamic Lighting]].

Shadow often come with a large performance penalty.
That is why there are so many special cases and specific solutions.

Shadows can be seen more easily in Unreal Editor by Viewport > Lit > Lighting Only.

# Normal Dynamic Shadows
Cast by e.g. Point Lights.
These are cached, meaning that they aren't that expensive as long as neither the light source nor the shadow caster moves.
They can still be Movable, just that they aren't actually moved.
(
I think both.
)

The caching can be toggled with the console command `r.Shadow.CacheWholeSceneShadows`.

# Cascaded Shadow Maps
Used for outdoor environments illuminated by a [[Directional Light]].
(
And maybe other types of light sources as well, not sure.
)
Don't want to render shadows across an entire Landscape, too high performance cost.
Need to distance cull shadows at some point.
Don't want them to just disappear, would be a visible pop, instead should disappear gradually.

Cascaded Shadow Maps are shadows at three, by default, different qualities used for different distances.
Set on [[Directional Light]], in Details panel > Cascaded Shadow Maps.

Still quite costly in terms of render times.

# Distance Field Shadows
Cache the shape of a geometry into a texture as a distance field.
The distance field has lower resolution/precision than the geometry.
Used mainly for distance shadows, far from the camera, where high precision isn't necessary.
Takes over after the last Cascaded Shadow Map.
Can be used for 20k or so units.
The distance fields can be visualized with Viewport > Show > Visualize > Mesh Distance Fields.
To use Distance Field Shadows you must enable Mesh Distance Fields in the Project Settings.
Comes with a memory cost. I assume VRAM.

# Inset Shadows
Also called Per Object Shadows.
A way to improve shadow quality for a particular thing.
Often used for characters.
Enabled by setting a Light to Stationary.
Renders a Per Object shadow on top of the baked shadow.

For a mesh you can enable Details panel > Lighting > Dynamic Inset Shadow.

# Contact Shadows
Fine detail shadows for two objects that are close to each other.
Cases where the other shadow methods are to imprecise.
Screen-space post-process method.
