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


## Far Shadows

A second set of cascaded shadow maps for distant objects, beyond the range of the Signed Distance Field Shadows.
(
Not sure on the relationship between Far Shadows and Signed Distance Field Shadows.
In [Rendering Kickstart - Rendering - Advice @ learn.unrealengine.com](https://learn.unrealengine.com/course/3537481/module/6853946) they say that Far Shadows are outdated and replace by Signed Distance Field Shadows.
)
Enabled on a [[Directional Light]] and on [[Static Mesh|Static Meshes]].
Enable Directional Light > Details panel > Cascaded Shadow Maps > Far Shadow Cascade Count and Far Shadow Distance.
Far Shadows are active between Dynamic Shadow Distance and Far Shadow Distance.
Enable Static Mesh > Details panel > Lighting > Advanced > Far Shadow.


# Capsule Shadows
Simplified version of some geometry to gain performance.
Only for skeletal meshes.
Skeletal meshes have simplified collisions approximating its shape.
That volume approximation can be used for shadowing instead of the triangles.
Enabled on a Skeletal Mesh > Details panel > Lighting > Capsule {Direct, Indirect} Shadow.

Useful when there are many small entities or where there aren't any intense lights.

# Distance Field Shadows
Cache the shape of a geometry into a texture as a distance field.
The distance field has lower resolution/precision than the geometry.
Used mainly for distance shadows, far from the camera, where high precision isn't necessary.
Takes over after the last Cascaded Shadow Map.
Can be used for 20k or so units.
The distance fields can be visualized with Viewport > Show > Visualize > Mesh Distance Fields.
To use Distance Field Shadows you must enable Mesh Distance Fields in the Project Settings.
Comes with a memory cost. I assume VRAM.

Should be turned off for tiny meshes because Distance Field Shadows are intended for distant objects and the shadow of small distant objects will be too small to be seen.
[[Static Mesh]] > Details panel > Lighting > Advanced > Affect Distance Field Lighting.
This should be set on the [[Static Mesh Asset]], not individual instances since it is very easy to miss one.
On the [[Static Mesh Asset]] set Distance Field Resolution Scale to 0.0 to completely disable Distance Field Shadow for it.
There are two Distance Field Resolution Scale, on under LOD 0 > Build Settings and one under Import Settings > Mesh.
Not sure how they differ.
(
Is there a way to set a size threshold globally?
)

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

# Raytraced Ambient Occlusion
Looks good and fast.

# Distance Field Ambient Occlusion
Needs a stationary or movable (not static) Sky Light.
Uses the geometry distance fields in the level to compute ambient occlusion.
Configured in Sky Light > Details panel > Distance Field Ambient Occlusion.
Used for large-scale ambient occlusion, mainly outdoor environments.
For example to make the inside of an opening to a cave dark even if there is no shadow active there.
Not precise, not for tiny objects/crevices.
Keep the Scale of all geometries using distance fields close to 1.0 along all axes.


# Cascaded Shadow Maps VS Distance Field Shadows

Cascaded Shadow Maps typically end 3'000-4'000 units from the camera.
Distance Field Shadows take over until 20'000 to 30'000 units or so.


# References

- [Rendering Kickstart - Rendering - Advice @ learn.unrealengine.com](https://learn.unrealengine.com/course/3537481/module/6853946)

