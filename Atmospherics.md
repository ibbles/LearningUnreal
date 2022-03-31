# Exponential Height Fog
Simplest, basically a flat color.
Fade fog in the distance and up.

Control the amount of fog with Details panel > Exponential Height Fog Component > Fog Density.
0.2 is a lot.

## Volumetric Fog
A setting on Exponential Height Fog.
A checkbox at Details panel > Volumetric Fog > Volumetric Fog.

A Volumetric Fog will read the Volumetric [[Lightmap]] and color itself.
It will therefor be shadowed by e.g. trees and buildings, i.e. produce god rays.

A Volumetric Fog will be illuminated by [[Light|Lights]], even dynamic ones.

Particles can create Volumetric Fog.
Not sure if this is Cascade, the legacy particle system, or [[Niagara]].
Apply a Material configured as a Volumetric Material to the particles.


# Atmospheric Fog
Legacy, superseded by Sky Atmosphere.

# Sky Atmosphere

