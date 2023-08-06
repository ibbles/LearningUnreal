The Content Browser is the [[Unreal Editor]] panel where we can see the [[Asset|Assets]] that has been imported into or created inside our project.
Many assets are created from the right-click menu in the Content Browser, either in an empty space or on an Asset that can be used as a parent for new assets.
Asset are imported using the Import button at the top-left of the Content Browser.
There is also an Add button in the top-left of the Content Browser where we can both import and create new assets.

The Content Browser is organized much like a file system, with folder and files.
A tree view can be opened on the left hand side of the Content Browser.
The Content Browser has multiple roots, i.e. places where it searches for assets.
The main one is named `Content`, this is the assets that are part of the currently open project.
There is also a `Plugins` folder for the assets that has been added to the project with a [[Plugin]].
There is also an `Engine` folder for assets that came with the Unreal Engine installation.

Assets that has a visible representation, such as [[Static Mesh Asset]], [[Texture]], and [[Sound]] get a preview icon / thumbnail when the Content Browser > Settings > View Type has been set to Tiles.
See _Thumbnail Edit Mode_ below.
Sometimes the preview isn't loaded, to force it right-click on the problematic asset.
Each asset type is associated with a color that is shown on the asset icon regardless of View Type.

Hovering the mouse cursor over an asset opens a pop-up with some type-specific information about the asset,
such as the number of vertices of a [[Static Mesh Asset]] or the size and channels of a [[Texture]].

Right-click an asset to get a list of actions that can be performed on that asset.
Some of the actions are described throughout this note.

We can move an [[Asset]] by selecting it and dragging it to the folder where we want it to be.
Unreal Engine tracks all asset references all will update them to point to the new location.

Unreal Engine 5 introduced the [[Content Drawer]].
The Content Drawer is an auto-hiding panel with a Content Browser in it.
We can open the Content Drawer with Ctrl+Space.
The Content Drawer can be docked by clicking the Dock In Layout button  in the top-right.
This turns it into a regular panel.


# Asset Filtering

The Content Browser can be filtered, i.e. made to show only certain types of assets.
At the top of the Content Browser, click the dashed triangle to the left of the search bar.
This opens the filter list, which is a list of asset types along with a check box next to each.
Select a few check boxes and only asset of the selected types will be shown.
Only  assets matching the filter within the currently selected folder, including subfolders, will be shown.

By default the text search searches by name.
It is also possible to search by meta-data.
Type the name of a property, such as UVChannels.
This forms a query, so we need to provide an expression.
For example `UVChannels>1`.
Multiple terms can be joined with `AND` and `OR`.
A search can be saved as a custom filter or as a Collection, see _Collections_ below.

It can be useful to open up multiple Content Browser panels and have different filtering settings for them.
Open new Content Browsers from [[Top Menu Bar]] > Content Browser.


# Collections

Collections are virtual folders.
A way to group [[Asset|Assets]] that has some relationship other than the folder hierarchy.
For example we may have a folder containing all the assets that define a vehicle grouped by asset type and collections to group everything that make up the doors.


# Settings

Accessed from the top-right corner of the Content Browser.
Switch between Tiles (preview thumbnails), List (names only), and Columns (detailed meta-data).
The column mode show different data depending on the type of assets in the current folder.
(
How does this behave when there are assets of different types within a single folder?
Is this a reason to keep assets separated by type?
)
The Shows Sources settings shows or hides the folder tree view to the left.


# Thumbnail Edit Mode

Some [[Asset|Assets]], such as  [[Texture]], [[Static Mesh Asset]], and [[Material]], will show a preview of the [[Asset]] in the Content Browser.
Some of those assets support customizing the preview.
For example, for a [[Static Mesh Asset]] we can control the point-of-view.
To enable Thumbnail Editing Mode do Content Browser > Settings > Thumbnail Edit Mode.
Click-and-drag on an [[Asset]] to orbit the camera.
Click Done Editing when done.
