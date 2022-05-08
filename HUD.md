HUD is short for Heads-Up Display.
It is the 2D elements that are rendered "on the camera lens"-ish.
Often used to display health bars, ammo counters, mini-maps, and other gameplay related state that the player need to have quick access to.

I don't know the difference between having a HUD vs adding a [[Widget Blueprint]] to the Viewport.

**Create a new HUD** Blueprint by Content Browser > right-click > Blueprint Class > type HUD in the search field > Click Select.
HUD Blueprints are often named with a `HBP_` prefix.

Make a particular HUD class the **active** one by selecting it in [[Game Mode]] > Details panel > Classes > HUD.
Make sure the [[Game Mode]] is selected in either the [[World Settings]] or the [[Project Settings]].

The HUD is owned by the [[Player Controller]], and if we have a reference to the [[Player Controller]] then we can get the HUD with the Get HUD node.

A HUD is a [[Blueprint Class]] and therefor has a Components list, a My Blueprint panel, a Viewport, and an [[Event Graph]].

HUD has a Blueprint Event named **Event Receive Draw HUD** which is given the width and the height of the screen.
The size is specified with integers so I assume they are in pixels, or [[DPI]] scaled pixels.
The screen size parameters are named `Sizex X` and `Size Y`.

Drawing elements on the HUD is done with various **Draw** nodes.
The Draw nodes are given Screen X and Screen Y coordinates.
I believe those are in pixels, with (0, 0) being the upper-left corner and (`Size X - 1`, `Size Y - 1`) being the lower-right corner.

A HUD can draw a [[Texture]] with the **Draw Texture** node.
The position where the [[Texture]] is drawn specified with floats.
The width (Screen W) and height (Screen H) of the rendered texture is specified in pixels.
Usually set to the size of the source texture, seen in the pop-up that is displayed when hovering over the [[Texture]] [[Asset]] in the [[Content Browser]].

A crosshair can be created by drawing a texture to the screen at
- `Screen X` = `Screen Size X * POS_X - Texture Size X * 0.5`
- `Screen Y` = `Screen Size Y * POS_Y - Texture Size Y * 0.5`
where `POS_X` and `POS_Y` are often 0.5, i.e. middle of the screen.
By saving the (`Screen Size X * POS_X`,  `Screen Size Y * POS_Y`) coordinate we can use that later to do [[Line Trace]].
Pass the screen coordinate of the crosshair to Player Controller > Deproject Screen To World.

# References
- [Creating a Block-Based Game - Adding the PlayerCharacter, Crosshair, and GameMode @ learn.unrealengine.com](https://learn.unrealengine.com/course/3770925/module/7308627)
