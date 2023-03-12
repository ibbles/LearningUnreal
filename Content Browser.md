The Content Browser is the [[Unreal Editor]] panel where we can see the [[Asset|Asset]] that has been imported into or created inside our project.
Many assets are created from the right-click menu in the Content Browser, either in an empty space or on an Asset that can be used as a parent for new assets.
Asset are imported using the Import button at the top-left of the Content Browser.

The Content Browser is organized much like a file system, with folder and files.
A tree view can be opened on the left hand side of the Content Browser.
The Content Browser has multiple roots, i.e. places where it searches for assets.
The main one is named `Content`, this is the assets that are part of the currently open project.
There is also a `Plugins` folder for the assets that has been added to the project as a [[Plugin]].
There is also an `Engine` folder for assets that came with the Unreal Engine installation.

Assets that has a visible representation, such as [[Static Mesh Asset]], [[Texture]], and [[Sound]] get an icon when the Content Browser > Settings > View Type has been set to Tiles.
Sometimes the preview isn't loaded, to force it right-click on the problematic asset.

We can move an [[Asset]] by selecting it and dragging it to the folder where we want it to be.
Unreal Engine tracks all asset references all will update them to point to the new location.

Called Content Drawer in Unreal Engine 5. Kinda. The Content Drawer is a type of auto-hiding Content Browser.
We can open the Content Drawer with Ctrl+Space.


# Thumbnail Edit Mode

Some [[Asset|Assets]], such as  [[Texture]], [[Static Mesh Asset]], and [[Material]], will show a preview of the [[Asset]] in the Content Browser.
Some of those assets support customizing the preview.
For example, for a [[Static Mesh Asset]] we can control the point-of-view.
To enable Thumbnail Editing Mode do Content Browser > Settings > Thumbnail Edit Mode.
Click-and-drag on an [[Asset]] to orbit the camera.
Click Done Editing when done.
