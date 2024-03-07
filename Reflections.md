Three main types, and ray-traced as a separate thing.
Stacked/chained so that if a cheaper method has already rendered a pixel then the more expensive methods won't run.

The reflections can be visualized with Viewport > Lit > Reflections.

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
Does not work well for mirror-like reflections.

# Planar Reflections
Very accurate for flat reflective surfaces, like polished floors.
Added manually.
Only works on flat surfaces.
Enable with Planar Reflection > Details panel > Rendering > Planar Reflection Component > Visible.
Can capture the surroundings every frame, but very costly for performance.

# Screen Space Reflections
Render reflections by reading other pixels that has already been rendered.
Can not read things that are not rendered, i.e. are out of view.
Always available, does not need to add anything to the level.
Tweak the quality of the Screen Space Reflections with a [[Post Process Volume]] or console commands.

# Raytraced Reflections
Layered on top of everything else.
Not sure if still available in Unreal Engine 5, they have changed and deprecated quite a bit of the Raytracing code.
