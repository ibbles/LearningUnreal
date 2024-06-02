A system to break up a large world, or [[Level]], into a grid of streamable cells.
The successor to [[World Composition]].

Must be enabled in [[Project Settings]] > Engine > World Partition > Enable World Partition.


# Grid Visualization

You can see how your [[Level]] has been split into cells by enabling [[World Settings]] panel > World Partition Setup > World Partition > Runtime Hash > Runtime Settings > Preview Grids.
A different grid is shown in the World Partition panel.
The World Partition panel always works at a size of 51'200 cm, or 512 m.
When loading a World Partition panel cell all grid cells within it will be loaded.

The [[Console Commands]] `wp.MiniMap.ShowReloadButton` will cause a Reload Mini Map button to appear above the grid in the World Partition panel.
Clicking this button renders a top-down view of the [[Level]] and creates a World Partition Mini Map [[Actor]] in the [[Level]] that holds a reference to a [[Texture]] where the mini-map is stored.


# Actor To Cell Assignment

An [[Actor]] is by default assigned to a cell based on its location.
This can be configured from [[Actor]] > [[Details Panel]] > World Partition > Grid Placement.
The options are:
- Bounds: The [[Actor]] is loaded when any cell its bounding volume overlaps is loaded.
- Location: The [[Actor]] is loaded when the cell that its location is in is loaded.
- Always Loaded: The [[Actor]] is always loaded. An always Loaded [[Actor]] is not placed in any cell.


# In-Editor Cell Loading And Unloading

The World Partition panel gives you an overview of how your [[Level]] is mapped to the grid of cells.
Load an area into the Unreal Editor by selecting the cells in the World Partition panel > right-click > Load Selected Cells.
Unload an area from the Unreal Editor by selecting the cells in the World Partition panel > right-click > Unload Selected Cells.
Loading and unloading cells is quick.


# Grid And Cell Configuration

The World Partition grid is configured in the [[World Settings]] panel, under World Partition Setup > World Partition > Runtime Hash.

There can be multiple grid.
One grid is often enough.
Each grid has:
- Name: The name of the grid.
- Cell Size: How large each cell is.
- Loading Range: I assume this is how close the player needs to be for the cell's contents to be loaded.


# Distance LODs

Related to [[HLOD]].
The [[Console Commands]] `wp.Runtime.HLOD 0` will hide all [[HLOD]] objects.

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

(
Is the following bit about HLOD specifically or World Partition in general?
)
We can preview  which cells are currently loaded with the [[Console Commands]] `wp.Runtime.ToggleDrawRuntimeHash[23]D 1`.
The 2D variant creates a UI widget showing a grid view with
- the player at the center,
- a line showing the view direction,
- a circle showing the range in which cells are loaded,
- green squares showing loaded cells,
- red squares showing unloaded cells.
- I don't know what the gray cells are.

# See Also

- [[Level Instance]]
- [[Packed Level Instance]]
- [[Data Layer]]


# References

- [_Large worlds in UE5: A whole new (open) world | Unreal Engine_ by Epic Online Learning 2021 UE5.0](https://dev.epicgames.com/community/learning/talks-and-demos/KBe/large-worlds-in-ue5-a-whole-new-open-world-unreal-engine)

