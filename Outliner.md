A panel in the [[Level Editor]] that shows all the [[Actor|Actors]] that exist within a [[Level]].
With Unreal Engine 5 this was renamed from [[World Outliner]].

The Outliner is a table with multiple columns.
By default the Visibility, Pinned, Label, and Type columns are enabled.
Enable or disable columns by right-clicking the column header row at the top.
You can also replace the Type column with other data.

Click a column title to sort by that column.
Click again to reserve the sorting.

The visibility toggle, the eye icon shown to the left when the Visibility column is enabled, only control visibility in the editor.
This setting is ignored when playing the game.
If multiple objects are selected then clicking any of the eyes will toggle all of the selected ones.
Use [[Details Panel]] > Rendering > Visible (Correct path?) to toggle rendering during gameplay.

The [[Actor|Actors]] are organized in a hierarchy where they can be placed either inside folders or as children to each other.
An [[Actor]] that is a child of another [[Actor]] will move with the parent [[Actor]].
(TODO Figure out how to make a Level Instance current.)

# Folders

Folders are created with the button in the top-right.
The folder is created next to or as a child of the currently selected item.
Deleting a folder only deletes the folder itself, any [[Actor]] within the folder is moved up to the parent folder,
or the root if there is no parent folder.
The root cannot be deleted.

Objects can be selected, renamed, grouped, and hidden with the eye icon.
Move to a folder by selecting and either dragging to the folder or right-click > Move To > select either a folder or Create New Folder.

A folder can be made current with right-click > _Actor Editor Context_ > Make Current Folder.
When an [[Actor]] is created when there is a Current Folder then the created [[Actor]] will be placed in that folder.
The current folder is shown in the bottom-right corner of the [[Level Viewport]].
To un-current a folder right-click any folder > _Actor Editor Context_ > Clear Current Folder och click the close x button on  the current folder widget in the lower-right corner of the [[Level Viewport]].

# Search And Filter

The search bar at the top can be used to filter the list of [[Actor|Actors]] by name and type.
Put a `+` in front of the search term to make it an exact match search,
instead of sub-word search.
Put a `-` in front of the search term to make it a inverted search, i.e. hide [[Actor|Actors]] that match.
Searches can be saved by clicking the circle-plus button to the right.
Filter by type lists using the triangle-lines drop-down menu to the left of the search box.
Nested types appears as toggle buttons when a group-type is selected in the filter drop-down.
The triangle-lines drop-down menu button get a red dot when type filtering is enabled.



# References

- [_Unreal engine Editor Fundamentals > World Outliner Panel_ by Epic Games @ dev.epicgames.com 2023](https://dev.epicgames.com/community/learning/courses/D95/unreal-engine-editor-fundamentals/4xpj/unreal-engine-world-outliner-panel)
- [_An In-Depth Look at Environment Artist Based Tools_ > _Organizing the World Outliner_ by Epic Onlne Learning @ dev.epicgames.com/courses 2021 UE 4.27]

