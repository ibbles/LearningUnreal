

# [_Make A Professional MainMenu And A Settings Menu In Unreal Engine 5_ by Unreal ART With Alireza @ yotube.com 2024](https://www.youtube.com/watch?v=NOEM8mVk2r0)

A main menu is often made as a [[Level]]  named `MainMenu` that contains UI [[Widget]]s.
The level can contain 3D objects to serve as a the background to the main menu.
The level should contain a [[Camera]] [[Actor]], called the UI camera that defines what part of the scene is visible behind the main menu.

[(1, 21:19)](https://youtu.be/NOEM8mVk2r0?t=1279)
To make the player see from the  UI camera's view we need to do some setup.
This is done in the [[Level Blueprint]].
Do Get Player Controller > Set View Target With Blend and pass the UI Camera to New View Target.

[(1, 22:52)](https://youtu.be/NOEM8mVk2r0?t=1372)
To prevent the player from moving the [[Pawn]] around the level we need to do some setup.
This is done in the [[Level Blueprint]].
Do Get Player Controller > Set Input Mode UI Only.

(TODO Add timestamp  here.)
The main menu is a [[Widget Blueprint]] inheriting from [[User Widget]].
It contains a Canvas Panel used to manage the  UI elements more efficiently.
The [[Canvas Panel]] contains a [[Grid Panel]].
[[Grid Panel]] has [[Details Panel]] > Fill Rules > Column Fill and Row Fill arrays.
The values for these are the width and height fraction that each column and row, respectively.
Widgets added to the Grid Panel can be moved around the grid with the arrows that are shown around the widget in the viewport.
Set Grid Panel > Details > Slot > Anchors to Fill and set all offsets to zero.
This makes the Grid Panel fill the Canvas Panel.
Create a single 1.0 filling row and two 0.5 filling columns.
This divides the screen in a left half and a right half.

Create a second Canvas Panel in the Grid Panel.
This will make it easier to control the menu items.
The inner Canvas Panel should have Details > Slot > {Horizontal Alignment, Vertical Alignment} set to Fill.
This will make it will the entire Grid Panel slot.

To add the game title to the menu, add a Text to the inner Canvas Panel.

Add a Vertical Box to hold the menu buttons.
Add as many Buttons as you need, each with a Text.
For each Text set > Details > Content > Text to what the button does.


[(1, 23:32)](https://youtu.be/NOEM8mVk2r0?t=1412)
A UI [[Widget]] is added to the screen from the [[Level Blueprint]] with the [[Create Widget]] node.




# References

- 1: [_Make A Professional MainMenu And A Settings Menu In Unreal Engine 5_ by Unreal ART With Alireza @ yotube.com 2024](https://www.youtube.com/watch?v=NOEM8mVk2r0)
