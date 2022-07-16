Water can be either volumetric, i.e. the camera is inside the water, or a surface, i.e. the camera is above the water.


# Simple Planar Water Surface

Based on [_Let's Create an Unreal Engine 5 Environment in ONE SITTING Ep.2 | 3D Livestream_ - Water, by pwnisher @ youtube.com](https://youtu.be/k77o5Ug41ek?t=1414).
Basically making a material that is reflective and chrome.

Create a flat surface, such as the Plane in the Place Actors panel, and size it to fit your water area.
Create a new [[Material]], for example named `M_Water`.
In the [[Material Editor]], right-click [[Material Output Node]] > Base Color and select Promote To Parameter. Set Details panel > Default Value to white.
Right-click [[Material Output Node]] > Metallic and select Promote To Parameter. Set Details panel > Default Value to 1.0.
Right-click [[Material Output Node]] > Specular and select Promote To Parameter. Set Details panel > Default Value to 1.0.
Right-click [[Material Output Node]] > Roughness and select Promote To Parameter. Set Detail panel > Default Value to 0.0.

This creates a highly reflective flat surface without waves or ripples.
Suitable for small shallow puddles of water indoors.


# Water Plugin

Since Unreal Engine 4.26 there is a  water meshing and rendering system plugin in Unreal.


# References
- [_Let's Create an Unreal Engine 5 Environment in ONE SITTING Ep.2 | 3D Livestream_ - Water, by pwnisher @ youtube.com](https://youtu.be/k77o5Ug41ek?t=1414)

