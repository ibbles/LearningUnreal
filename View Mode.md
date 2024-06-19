A View Mode is a visualization tool that can show various aspects of the [[Rendering Pipeline]] in the [[Level Viewport]].
By default the Level Viewport will render the level much like it would look in-game, optionally with some extra billboards to show the location of non-rendered entities such as [[Light Source|Light Sources]] and [[Sound]] emitters.
This game-like view is called the Lit View Mode.
The billboards can be toggled with the `G` button, hiding them is called [[Game View]].
The [[Level Viewport]] can visualize other things as well.
These are called View Modes and are accessed from one of the buttons in the top-left part of the viewport.
The button will display the currently selected View Mode, which by default is Lit.
Clicking this button gives you a list of the available View Modes.

View modes are an artist-friendly way to understanding the performance complexities of the scene.
View modes can visualize the various types of rendering methods being used in the scene.

You can read more about the performance related View Modes in [[Rendering Performance Profiling And Analysis]].
You can read more about the lighting related View Modes in [[Lighting View Modes]].
You can read more about material related View Modes in [[Material View Modes]].

- Player Collision: Visualize the [[Collision Shape|Collision Shapes]] that the player will collide with.
- Buffer visualization
	- Custom Stencil. Shows the stencil buffer values.

# References

- [_Begin Play | Rendering_ 15:39 by Epic Games @ dev.epicgames.com 2022](https://dev.epicgames.com/community/learning/tutorials/vyZ1/unreal-engine-begin-play-rendering)
- [_Becoming an Environment Artist in Unreal Engine_ > _Collison Setup_ by Epic Online Learning @ dev.epicgames.com/courses 2022 UE4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/jMJ/unreal-engine-collison-setup)
