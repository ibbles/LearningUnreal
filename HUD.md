HUD is short for Heads-Up Display.
Is is the 2D elements that are rendered "on the camera lens"-ish.
Often used to display health bars, ammo counters, mini-maps, and other gameplay related state that the player need to have quick access to.

I don't know the difference between having a HUD vs adding a [[Widget Blueprint]] to the Viewport.

**Create a new HUD** Blueprint by Content Browser > right-click > Blueprint Class > Type HUD in the search filed > Click Select.
HUD Blueprints are often named with a `HBP_` prefix.

Make a particular HUD class the **active** one by selecting it in [[Game Mode]] > Details panel > Classes > HUD.
Make sure the [[Game Mode]] is selected in either the [[World Settings]] or the [[Project Settings]].

The HUD is owned by the [[Player Controller]], and if we have a reference to the [[Player Controller]] then we can get the HUD with the Get HUD node.

A HUD is a [[Blueprint Class]] and therefor has a Components list, a My Blueprint panel, a Viewport, and an [[Event Graph]].

HUD has a Blueprint Event named **Event Receive Draw HUD** which is given the width and the height of the screen.
The size is specified with integers so I assume they are in pixels, or [[DPI]] scaled pixels.

A HUD can draw a [[Texture]] with the **Draw Texture** node.
The position where the [[Texture]] is drawn specified with floats.
The width (Screen W) and height (Screen H) of the rendered texture is specified in pixels.
Usually set to the size of the source texture, seen in the pop-up that is displayed when hovering over the [[Texture]] [[Asset]] in the [[Content Browser]].


# References
- [Creating a Block-Based Game - Adding the PlayerCharacter, Crosshair, and GameMode @ learn.unrealengine.com](https://learn.unrealengine.com/course/3770925/module/7308627)
