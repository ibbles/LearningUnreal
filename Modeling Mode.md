The modeling tools are available from the Modeling [[Editor Mode]].
Switching to the Modeling Mode opens two panels.
One, the Mode Toolbar panel, contains a collection of tools for objects we can create and operations we can perform on the created objects.
The other, the Modeling panel, contains  settings for the currently selected tool.


# Creating Shapes
The Shapes group in the Mode Toolbar panel contains tool for creating basic shapes.
Selecting one of them will populate the Modeling panel with settings for that tool.
For example, selecting the Box tool will show a bunch of box-related settings.

Moving the mouse cursor over the [[Level Viewport]] with one of the object creation tools selected will show the object and clicking will create it.
A button labeled `Complete` at the bottom of the Viewport unselects the currently selected tool.

# Auto Generated Assets
Each created object will also create an [[Asset]] to hold the data for that object.
The asset is either created relative to the currently open [[Level]], in a single folder for all Levels, or in the current folder.
There are settings one can set related to this in Project Settings > Plugins > Modeling Mode.
By setting
- asset Generation Location to Auto Generated Global Asset Path
- auto Generated Asset Path to a path relative to the Content folder
- disabling Use Per User Autogen Subfolder

we get a single folder where all created assets are placed.


# References

- [Exploring Geometry Tools in UE5 | Inside Unreal by Unreal Engine @ youtube.com](https://youtu.be/apCSgAAkDTU?t=506)

