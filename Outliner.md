A panel that shows all the [[Actor|Actors]] that exist within a [[Level]].
With Unreal Engine 5 this was renamed from [[World Outliner]].

The Outliner is a table with multiple columns.
By default the Visibility, Pinned, Label, and Type columns are enabled.
Enable or disable columns by right-clicking the column header row at the top.

The visibility toggle, the eye icon shown to the left when the Visibility column is enabled, only control visibility in the editor.
If multiple objects are selected then clicking any of the eyes will toggle all of the selected ones.
This setting is ignored when playing the game.
Use Details panel > Rendering > Visible (Correct path?) to toggle rendering during gameplay.

The [[Actor|Actors]] are organized in a hierarchy where they can be placed either inside folders or as children to each other.
An [[Actor]] that is a child of another [[Actor]] will move with the parent [[Actor]].

Folders are created with the button in the top-right.
The folder is created next to or as a child of the currently selected item.
Deleting a folder only deletes the folder itself, any [[Actor]] within the folder is moved up to the parent folder,
or the root if there is no parent folder.
The root cannot be deleted.

Objects can be selected, renamed, grouped, hidden with the eye icon.
Move to a folder by selecting and either dragging to the folder or right-click and select either a folder or Create New Folder.


# Search And Filter

The search bar at the top can be used to filter the list of [[Actor|Actors]] by name and type.
Searches can be saved by clicking the circle-plus button to the right.
Filter by type lists using the triangle-lines drop-down menu to the left of the search box.
Nested types appears as toggle buttons when a group-type is selected in the filter drop-down.
The triangle-lines drop-down menu button get a red dot when type filtering is enabled.
