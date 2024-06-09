The modeling tools are available from the Modeling [[Editor Mode]].
Switching to the Modeling Mode opens two panels.
One, the Mode Toolbar panel, contains a collection of tools for objects we can create and operations we can perform on the created objects.
The other, the Modeling panel, contains  settings for the currently selected tool.

All models in Unreal Engine are triangle meshes.

The organization of the tabs and tools seems to have changed quite a bit between Unreal Engine versions
The following listing is a mix that will, hopefully, be cleared up over time as Epic Games settles on a particular organization.


# Shapes

The Shapes group in the Mode Toolbar panel contains tool for creating basic shapes.
Selecting one of them will populate the Modeling panel with, among other things, settings for that shape.
For example, selecting the Box tool will show a bunch of box-related settings.

Moving the mouse cursor over the [[Level Viewport]] with one of the object creation tools selected will show the object and clicking will create it.
A button labeled `Complete` at the bottom of the Viewport unselects the currently selected tool.

Two things are created:
- A [[Static Mesh Asset]] holding the triangle data.
- An [[Actor]] with a [[Static Mesh Component]] referencing that asset.

We can reuse the asset for other purposes as well, for example in other Actors.
Editing the mesh with the Modeling Mode edit tools will edit the asset and therefore update everything that uses it,
not just the originally created [[Actor]].

This bears repeating.

**Editing a mesh in the Modeling Mode will edit the [[Static Mesh Asset]] and therefore edit all places where that asset is used.**

If you are editing a mesh then make sure that you either want to edit multiple use location, or that the thing you are editing through is the only user of the underlying [[Static Mesh Asset]].
I'm not sure how to quickly determine that.
The slow way is through the [[Reference Viewer]].

The Modeling panel contains Shape > **Polygroup Mode** which control how the PolyModel tools will interact with the mesh.
With Per Face the tool will create polygroups containing many quads from what constitutes a "face" in the selected mesh.
For example, for a cube the faces are the six faces of the cube, for a cylinder there is one face around the curvature and one each for the top and bottom, a sphere is divided up in to six polygroups kinda like a cube.
With Per Quad we get one polygroup per pair of triangles.

## Physics and Collisions

Shapes created in the Modeling Mode will have collisions enabled but will not simulate physics by default.
The raw triangles will be used for collision detection since Details panel > Collision > Collision Complexity is set to Use Complex Collision As Simple on the created Static Mesh Asset, and no collision primitives are created.
However, see Volumes > Msh2Coll below for ways of overriding this default.

Actors created in the Modeling Mode are not simulated even after enabling Details panel > Physics > Simulate Physics.
I don't know why.


## Auto Generated Assets

Each created object will also create an [[Asset]] to hold the data for that object.
The asset is either created relative to the currently open [[Level]], in a single folder for all Levels, or in the current folder.
There are settings one can set related to this in Project Settings > Plugins > Modeling Mode.
By setting
- Asset Generation Location to Auto Generated Global Asset Path,
- Auto Generated Asset Path to a path relative to the Content folder, and
- disabling Use Per User Autogen Subfolder
we get a single folder where all created assets are placed.


# Create

## MeshMerge

Merge the selected Static Meshes into a single new [[Static Mesh Asset]].
Not sure how this relates to PolyModel > MeshBool > Union and  MeshOps > Merge.

## MshDup - Mesh Duplicate

Create a copy of the currently selected [[Static Mesh Asset]].
Beware of the Handle Inputs > Delete Inputs setting.
Don't want to accidentally delete assets.

# XForm

## Merge

Used to merge multiple [[Static Mesh]]es into a single one.
Select multiple [[Static Mesh Actor]]s in the [[Level]].
Click Merge, click Accept.
The selected [[Static Mesh Actor]]s are replaced by a single one using a generated [[Static Mesh Asset]] with all the triangles (I think.) from the source meshes.
The settings window > On Tool Accept > Handle Inputs setting lets you decide what to do with the original [[Static Mesh Actor]]s.
- Hide Inputs: Keep the source [[Static Mesh Actor]]s in the [[Level]] but hide them, i.e. close the eye in the [[Outliner]]. Not sure if this also disables the Visible [[Property]] on the [[Static Mesh Component]].

# PolyModel

## PolyEdit

Used for editing groups of triangeles.
The grouping is defined by the Polygroup Mode selected when creating the shape.
Provides tools for this such as transform, extrude, insert edge loops, and more.
Can chose to select vertices, edges, and faces.

### Face Edits

### Shape Edits

#### Insert Edge Loop

Select the Insert Edge Loop and click and edge to cut the mesh along that edge where you clicked.

Set the Position Mode to Even to create multiple cuts along the selected edge.
Selecting Even enables the Num Loops setting, which should be set to the number of cuts to create.

**Insertion Mode** control how the edge loop is created.
- Plane Cut: Vertices are created where a plane perpendicular to the edge cuts through the mesh. Existing edges remain.
- Retriangulate: Recreate triangles for the two halves after the cut. Good for meshes consisting of square polygroups.

### Edge Edits

#### Bevel Edges
- Set the selection checkboxes to edges only.
- Select all edges using click-and-drag over the entire mode.
- Click Modeling panel > Edge Edits > Bevel.
- Edit the settings to your liking.
- Click Apply at the top of the Modeling panel.

The edges will be split in two, plus some extra near the vertices.

## MshBool - Mesh Boolean

Perform boolean operations between meshes.
Must have two meshes selected for this tool to be available.

### Difference
### Union
Merge two meshes into a single mesh.
[[Material Slot|Material Slots]] that have the same material are merged.
Material Slots from the second mesh that points to a material not used by the first mesh is placed after the material slots of the first mesh.
You can use Mode Toolbar > Attributes > MatEd to add, remove, and reassign material slots to triangles if the auto-assignment didn't do what you want.



# TriModel

The TriModel part of the Model Toolbar is for editing the model at the triangle level.
A Static Mesh must be selected in the Level Editor for these tools to activate.

## TriSelet
For seleting triangles.
I haven't been able to figure out how to use this tool yet.
I can select triangles, they are marked in red, the the commit button remain grayed out.
Red is a warning color, why are the selected triangles red?

## TriEdit

Select and the move and/or rotate single or groups of vertices, edges, and triangles.
You can select multiple vertices with click-and-drag.
Hold Ctrl while dragging to remove from the selection.
Hold Shift while dragging to add to the selection.
Click Accept at the bottom of the Viewport to finalize the edit.

Does not use the regular Transform Gizmo, so the regular keyboard and mouse controls doesn't work.
The world / local toggle button at the top-right of the Viewport does work.

## PlaneCut

Cuts a mesh using a plane.
Use the Transform Gizo to place the cut plane and then either click Accept at the bottom of the Viewport to finalize the cut or Cut in the Modeling panel to perform this cut and prepare to make another.

The plane as a direction, a keep side and a discart side.
Click Flip Plane to swap the two directions.


# Deform

## VSculpt - Vertex Sculpting

Move vertices around similar to when [[Sculpt a Landscape|sculpting a Landscape]].

## DSculp - Dynamic Sculpting

Move vertices around similar to when [[Sculpt a Landscape|sculpting a Landscape]].
Creates new vertices dynamically to maintain a target triangle density. (I think.)
Useful for cleaning up Landscape-like meshes that have gone bad.
Can also use MeshOps > Remesh for this.

## Lattice

Place a 3D lattice over the model and deforming the lattice by selecting and dragging the control points at the lattice corners will distance weight blend nearby vertices.
Can have different Interpolation T... (Type?) to control the weighting function, i.e. how vertices progressively farther and farther away from the control point is affected when the control point is transformed.
- Linear: Gives straight lines between control points.
- Cubic: Gives curves between control points.

# Transform

## Pivot

### Center
Move the vertices so that the pivot point end up in the center of the mesh.
Not sure what "center" means here. Center of the bounding box? Center of the bounding sphere? Some kind of weighted average?

## BakeRS - Bake Rotation And Scale Into The Static Mesh Asset

Apply the Static Mesh Component's rotation and scale into the triangles, and reset the rotation and scale to unity.
I though that this would produce rendered geometry in the level that looks the same but with a 1:1:1 scale on the Component/Actor, but in my testing I've seen the geometry change shape.
There is something I don't understand.
I:
- Created a Plane using Matin Tool Bar > Add > Shapes > Plane.
- Scaled the plan on the X axis using the scale tool, making it longer.
- Applied a Quixel Megascans material and noticed severe stretching.
- Duplicate the Static Mesh Asset with Modeling Mode > Create > MshDup.
- Selected Modeling Mode > Transform > BakeRS.

Doesn't matter which settings I have for BakeRS, the plane always end up very long.
Selecting another Actor, then back to my plane again and clicking the reset arrow button next to Transform > Scale produced the correct scale again.

# Attributes

## MatEd - Material Editor

Used to add, remove, and reassign [[Material Slot|Material Slots]].

**Adding and removing Material Slots** is done from Modeling panel > Materials > Materials, which is an array.

**Assigning material slots to triangles** is done by selecting both the Material Slot and the triangles.
Click in the Viewport or use one of Selection Edits > Select All button to select triangles.
Select a Material Slot at Materials > Active Materials.
Click Material Edits > Assign Active Material to assign the active Material Slot to the selected triangles.


# UVs
Tools for manipulating the texture coordinates.
These tools are often used in combination with Top Menu Bar > Actor > Asset Tools > [[UV Editor]].


## AutoUV
Automatically compute decent texture coordinates.
If the material remain stretched after AutoUV then consider baking any Actor scale into the mesh using the Transform > BakeRS tool.

# Baking

# Volumes

## Msh2Coll - Mesh To Collision

Generates collision shapes for the selected meshes.

A decent setup is 
- Options > Geometry Type > Convex Hulls.
- Options > Input Mode > Per Mesh Component.
- Output Options > Append To Existing > Enable.
	- Not sure about this one.


# Uncertainties And Questions

What is the similarities and differences between the following:
- Create > MshMrg.
- PolyModel > MeshBool > Union.
- MeshOps > Merge.
- XForm > Merge.

Which of the above are renames of the same thing?

# References

- [Exploring Geometry Tools in UE5 | Inside Unreal by Unreal Engine @ youtube.com](https://youtu.be/apCSgAAkDTU?t=506)
- [_Introduction to PCG Workflows in Unreal Engine 5 | Unreal Fest 2023_ by Epic Games @ youtube.com 2023](https://www.youtube.com/live/LMQDCEiLaQY)

