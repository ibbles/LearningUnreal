Three types.
Stacked/chained so that if a cheaper method has already rendered a pixel then the more expensive methods won't run.

# Reflection Captures
The cheapest reflection type.
Sphere Reflection Captures are placed in the level.
Place big ones in the middle of each room, to act as a general fallback.
Place small ones near very reflective things.
A cube map is captured at each Sphere Reflection Capture.
A Sphere Reflection Capture has a radius.
Pixels within the radius will blend in the cube map captured at the Sphere Reflection Capture.
Fades in the blend based on distance from the Sphere Reflection Capture.
Not very precise, especially near the edge of the influence radius.

# Planar Reflections

# Screen Space Reflections