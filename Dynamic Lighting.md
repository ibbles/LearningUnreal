Dynamic Lighting is [[Lighting]] that is computed every frame.
This means that all [[Light Source Properties]] of the [[Light Source]] can be altered at runtime by game logic.
It is also possible to spawn and destroy lights at runtime.
Dynamic lights will produce shadows that move together with mesh animation, both [[Skeletal Mesh]] and [[Static Mesh]] with a [[Material]] that makes use of [[World Position Offset]].
A [[Light Source]] is made dynamic by setting its [[Mobility]] to Movable.

There is no [[Baked Lighting|baking]] process.

A Dynamic light will be rendered in Unreal Editor the same way as it will in-game.

Each feature, such as [[Dynamic Shadows]], is made up of many parts, sometimes separate, sometimes coordinated.
Different solutions doing very specific things.

Dynamic Lighting is consists of Direct Lighting and Indirect Lighting.
Direct Lighting is when  light move directly from a light source to an illuminated surface.
Indirect Lighting is when light bounce off of a surface, taking on some of the properties of that surface, and then illuminate a surface.
Indirect Lighting may bounce multiple times.
In Unreal Engine 5 indirect lighting can be computed by [[Lumen]].

We can force all light to be dynamic, i.e. not precomputed, by enabling World Settings > Lightmass > Advanced > Force No Precomputed Ligthing.

# IES Profiles
Adds a bit of structure to the light, the different patterns of weak and strong light.
Can be used to make the light match the shape and occlusion of a light fixture without having to use actual shadows, translucency, and refraction.
Make the ideal light source provided by Unreal less idealized.
Found in a Light's Details panel > Light Profile > IES Texture.
1D texture using a standardized format.
Not only for dynamic lighting, can be used for static lighting.

# Light Functions
Blend a Material with the light.
Set in a Light's Details panel > Light Function > Light Function Material.
Is a regular material with the Material Domain set to Light Function and with an Emissive Color output.
(
It may be possible to add other Material Outputs as well. Not sure.
)
Used for fire flickering, water caustics, or shadows from clouds.
The Details panel > Light Function > Fade Distance is important for performance.

# Light Propagation
Older real time Global Illumination system.
Not recommended for production use.

# Raytraced Global Illumination
High performance cost.
Replaced by Lumen.


# References

- [_Lighting Essential Concepts and Effects - Dynamic Lighting - Indoor and Basics_, by Epic Games @ dev.epicgames.com](https://dev.epicgames.com/community/learning/courses/Xwp/lighting-essential-concepts-and-effects/mX9k/dynamic-lighting-indoor-and-basics)


