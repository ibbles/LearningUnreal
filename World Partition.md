An automatic data management and distance based level streaming system.
A system to break up a large world, or [[Level]], into a grid of streamable cells.
Cells, and the [[Actor]]s within, that are close to the player, or any other Streaming Source, are streamed in.
Previously loaded cells that become not close are streamed out, i.e. unloaded.
What "close" means is defined by the grid's Loading Range.
The successor to [[World Composition]].

Must be enabled in [[Project Settings]] > Engine > World Partition > Enable World Partition.
(
This setting is not in in Unreal Engine 5.2.
Was it removed, or not yet added?
Seems to have been removed, not in Unreal Engine 5.4 either.
)
Is enabled per [[Level]] in [[World Settings]] panel > World Partition > Enable Streaming.
(
This setting does not exist in Unreal Engine 5.2 for [[Level]]s created from the Basic or Empty [[Level]] templates, or from the [[Content Browser]].
There is [[World Settings]] panel > World Partition Setup > World Partition, but it is set to None and cannot be edited.
If I create a new [[Level]] and select the Empty Open World template then [[World Settings]] panel > World Partition Setup does contain Enable Streaming.
Is it not possible to enable World Partition on a level not created from an Open World template?
If I create a [[Level]] from the [[Content Browser]] the it is created without World Partition
)

Each non-empty cell becomes a streaming level.

The automatic streaming only happens at runtime.
While in the Unreal Editor we are in control of which cells are currently loaded or not.
This is controlled from the World Partition panel.
See _In-Editor Cell Loading And Unloading_ below.


# Use Cases

Designed for large open worlds.

Comes with some overhead.
Should not be used for small levels that don't need streaming.


# Grid Visualization

You can see how your [[Level]] has been split into cells by enabling [[World Settings]] panel > World Partition Setup > World Partition > (Runtime Hash?) > Runtime Settings > Preview Grids.
This will render lines in the world showing the cell boundaries.
(
Different videos have this at different locations,
seems it has moved between versions.
Use the search bar at the top to find `Preview Grids`.
)

A different grid is shown in the World Partition panel.
The World Partition panel always works at a size of 51'200 cm, or 512 m.
When loading a World Partition panel cell all grid cells within it will be loaded.

We can preview  which cells are currently loaded with the [[Console Command]] `wp.Runtime.ToggleDrawRuntimeHash[23]D 1`.

The 2D variant creates a UI widget showing a grid view with cells of different colors
- the player at the center,
- a line showing the view direction,
- a circle showing the range in which cells are loaded,
- green squares showing loaded visible cells,
- red squares showing unloaded cells,
- yellow squares showing cells being loaded,
- light blue squares showing loaded but still hidden cells,
- I don't know what the gray cells are.

The 3D variant renders the cells in the 3D-world as colored transparent boxes.

# Generate Mini Map

The [[Console Command]] `wp.MiniMap.ShowReloadButton` will cause a Reload Mini Map button to appear above the grid in the World Partition panel.
Clicking this button renders a top-down view of the [[Level]] and creates a World Partition Mini Map [[Actor]] in the [[Level]] that holds a reference to a [[Texture]] where the mini-map is stored.
There is also a menu entry at [[Top Menu Bar]] > Build > _World Partition_ > Build Minimap that I believe does the same thing.

For large levels generating the mini map can take a long time.
This can be done with a [[Commandlet]].
Template: `unreal_executable project_name map_name tasks arguments`
```
/unreal/Engine/Binaries/Linus/UnrealEditor \
/projects/MyProject/MyProject.uproject \
/Game/MyProject/Maps/MyMap \
-Run=WorldPartitionBuilderCommandlet \
-AllowCommandletRendering \
-Builder=WorldPartitionMiniMapBuilder
```


# Actor To Grid And Cell Assignment

Each [[Actor]] has [[Details Panel]] > World Partition > Runtime Grid.
This controls which grid the [[Actor]] should be placed in one of the cells of.
The name entered here must match one of the grid names in [[World Settings]] > World Partition Setup > World Partition > Runtime Hash.
(I think that path has changed over time, check what it is now.)

An [[Actor]] is by default assigned to a cell based on its location.
This can be configured from [[Actor]] > [[Details Panel]] > World Partition > Grid Placement.
The options are:
- Bounds: The [[Actor]] is loaded when any cell its bounding volume overlaps is loaded.
- Location: The [[Actor]] is loaded when the cell that its location is in is loaded.
- Always Loaded: The [[Actor]] is always loaded. An always Loaded [[Actor]] is not placed in any cell.

Actors can be placed in secondary grids.
(What is a "secondary grid"? When should I use one?)

An [[Actor]] is only considered for streaming if [[Actor]] > [[Details Panel]] > ??? > Is Spatially Loaded is enabled.
If Is Spatially Loaded is false then the [[Actor]] is always loaded.
This is common for [[Directional Light]], [[Height Fog]], (more?).


# In-Editor Cell Loading And Unloading

The World Partition panel gives you an overview of how your [[Level]] is mapped to the grid of cells.
Load an area into the Unreal Editor by selecting the cells in the World Partition panel > right-click > Load Selected Cells.
Unload an area from the Unreal Editor by selecting the cells in the World Partition panel > right-click > Unload Selected Cells.

You can designate volumes that you often load and unload as a single unit.
For example a town.
These volumes are designated by creating a Location Volume [[Actor]].
The Location Volume is visualized as a wireframe box in the [[Level Viewport]].
Scale the volume to cover the area.
Give the [[Actor]] a name in the [[Outliner]].
Save the [[Level]].
The volume will now show up, including the name, in the World Partition panel.
To load all cells in the volume do World Partition panel > volume > right-click > Load Selected Regions.
To unload all cells in the volume do World Partition panel > volume > right-click > Unload Selected Regions.

You can pin an [[Actor]] while working in the editor.
A pinned [[Actor]] will not be unloaded.
Pin an [[Actor]] from the pin column in the [[Outliner]], next to the visibility eye.
Pinning is useful when working on something in a cluttered environment.
Pin the relevant [[Actor]]s and then unload all cells.
Only the pinned [[Actor]]s will remain in the [[Level Viewport]], making them easier to work with.

# Grid And Cell Configuration

The World Partition grid is configured in the [[World Settings]] panel, under World Partition Setup > World Partition > Runtime Hash.

## Multiple Grids

There can be multiple grids.
One grid is often enough, so start with just one.

One reason to have multiple grids is when a [[Level]] has wildly varying content densities across it,
for example a barren wasteland with towns scattered around.
Things in the barren wasteland part go into a grid with large cells,
while things in the towns go into a grid with smaller cells.
We can then balance things so that about the same number of objects are being assigned to each cell.

One reason to have multiple grids is for alignment, when you want to match some collection of objects, such as a town, in one part of the [[Level]] to the cell positions without messing up other places.
Then create a new grid for that particular town and set the origin of that grid so that the cell [[Actor]] cell assignment is what you want it to be.

One reason to have multiple grids is to control the loading range for different scaled things.
Small foliage and clutter can be placed in a grid with smaller Cell Size and a smaller Loading Range, while larger objects are placed in a grid with larger Cell Size and and a larger Loading Range.
This makes it so that we can have a larger variety and number of the smaller objects and foliage since a smaller area of them will be loaded, while the larger objects are still loaded farther away from the Streaming Source.
Think cutlery > furniture > signs > trees > buildings.

## Grid Settings

Each grid has:
- Name: The name of the grid.
- Cell Size: How large each cell is, measured in centimeters.
	- This can often be quite large, around 25600 cm.
- Loading Range: I assume this is how close the player needs to be for the cell's contents to be loaded, and how far the player needs to get for the cell's content to be unloaded.
- Block On Slow Streaming:
- Origin: Where the (0, 0) corner is.

Cell Size and Loading Range should be based on requirements on:
- Gameplay
- Physics
- Actor count.
(Don't know how though. What are the criteria?)

Don't be afraid to tweak the grid settings during production,
once you start play testing and can see the effects of different choices.

# Streaming Source

The Streaming Source decides which cells are to be loaded or unloaded.

There is a World Partition Streaming Source Component.
There is one on the Player Controller.
It has [[Details Panel]] > Streaming > Streaming Source Enabled.

When enabled the Streaming Source Component keeps track of where it is, which grids it is in, and what cells in those grids that are within each grid's Loading Range.
Those are the cells that should be loaded.

The Player Controller is one Streaming Source, bringing in cells round it.
When editing a Player Controller (Or any [[Actor]]?) we have the [[Blueprint Class Editor]] > [[Class Defaults]] > [[Details Panel]] > World Partition category that contains settings related to World Partition.
- Enable Streaming Source: Whether the Player Controller should be used as a World Partition Streaming Source.
	- If this is off then I don't think the player will bring in cells around it, the cells must we loaded some other way.
	- Is these an API for explicitly loading cells?
	- Are we limited to moving World Partition Streaming Source Components around?
- Streaming Source Should Activate: Whether the Player Controller Streaming Source should activate cells after loading.
	- I think "activating" means "make appear in the world", as opposed to being ready to appear at a moments notice.
- Streaming Source Should Block On Slow Streaming: Whether the Player Controller streaming source should block on slow streaming.
	- I assume a "block" in this case means blocking the game thread, i.e. we get a hitch if streaming isn't fast enough. So called "traversal stutter". Or just a long freeze if there is a lot to load, for example after a teleport. In that case we may want to show a fade or loading screen or something like that. How do we show a loading screen?
- Runtime Grid:
	- The Grid Name from [[World Settings]] panel > Runtime Settings > Grids > array element > Grid Name.
	- Surprising to me that this can only be a single name. What if we have multiple grids?


# Preload Teleporting

Teleporting can be done by simply moving the [[Player Controller]] / [[Pawn]] to the new location,
but if that causes a bunch of now in-range cells to trigger a bunch of [[Asset Loading]] then there will be a hitch in the frame rate.
A recommendation is to have something at the teleport target location with a Streaming Source Component and a few seconds before the teleport happens set Enable Streaming Source to true on that Streaming Source Component.
That will give the engine some time to preload the assets that will be needed after the teleport and the hitch will be minimized.


# Distance LODs

Related to [[HLOD]].
The [[Console Command]] `wp.Runtime.HLOD 0` will hide all [[HLOD]] objects.

World Partition can generate HLOD models for us.
[[World Settings]] panel > World Partition > HLOD > Default HLOD Layer is a reference to an asset.
This asset contains settings for the [[HLOD]].
- Level Type:
	- Instancing: Groups all of the same meshes into a single lowest-level [[LOD]] instance.
	- Simplified Mesh: Group the meshes together into clusters and generate a new single-[[Material]] simplified [[Mesh]] of each cluster. The Proxy Settings that appear when selecting Simplified Mesh contains a bunch of settings for the generated cluster [[Mesh]].
	- Merged Mesh:
- Always Loaded: Keep the [[HLOD]] mesh loaded even when the cell is unloaded.

The default [[HLOD]] generation settings selected in the [[World Settings]] panel can be overridden per [[Actor]] with [[Actor]] > [[Details Panel]] > [[HLOD]] > HLOD Layer.

With all the [[HLOD]] configuration setup  done the actual [[HLOD]] generation is done with a [[Commandlet]].
```
$ $UE_BINARY \
	MyProject.uproject \
	MyMap.umap \
	-Run=WorldPartitionBuilderCommandlet \
	-AllowCommandletRendering \
	-Builder=WorldPartitionHLODsBuilder
```

[[HLOD]] generation is done with a [[Commandlet]] instead of with a button in Unreal Editor to make it possible to run the generation as a background job on a build machine.


# Things To Test

What happens if a mechanism is partly in one cell and partly in another, then once of the cells is unloaded, or only one of the cells become loaded?
Will the mechanism break?
What happens if a constraint has one body in one cell and the other body in the other?
Will the constraint break?
What happens in one object rests on other that has its origin in another cell and is therefor unloaded but the on-top object remain active?


# See Also

- [[Level Instance]]
- [[Packed Level Actor]]
- [[Data Layer]]


# References

- [_Large worlds in UE5: A whole new (open) world | Unreal Engine_ by Epic Online Learning 2021 UE5.0](https://dev.epicgames.com/community/learning/talks-and-demos/KBe/large-worlds-in-ue5-a-whole-new-open-world-unreal-engine)
- [_Building Open Worlds in Unreal Engine 5 | Unreal Fest 2022_ by Epic Online Learning @ dev.epicgames.com/talks-and-demos](https://dev.epicgames.com/community/learning/talks-and-demos/LLM5/building-open-worlds-in-unreal-engine-5-unreal-fest-2022)
- [_Building Bigger: Changing Your Workflow for Building Worlds instead of Scenes | Unreal Fest 2023_ by Chris Murphy @ dev.epicgames.com/talks-and-demos 2024 UE5.3](https://dev.epicgames.com/community/learning/talks-and-demos/jwlJ/unreal-engine-building-bigger-changing-your-workflow-for-building-worlds-instead-of-scenes-unreal-fest-2023)
- [_World Partition_ by  Epic Games @ dev.epicgames.com/documentation](https://dev.epicgames.com/documentation/en-us/unreal-engine/world-partition-in-unreal-engine)

