Mesh LOD means that there are multiple versions of a [[Static Mesh]] (or [[Skeletal Mesh]]?) of varying complexity and depending on the distance or size on screen the renderer selects an appropriate version.
The purpose is to improve performance by not rendering more detail than necessary, which is important for [[Static Mesh Performance]].
Unreal uses screen size to pick the LOD level to use.
[[Nanite]], the new mesh management system introduce with Unreal Engine 5, replaces the LOD system described here.

LOD levels are either generated in a [[DCC]] software, such as Blender, and imported to Unreal, or generated from within Unreal Editor.
See _LOD Generation_ below.

# LOD Levels

We can see the LOD levels available to a [[Static Mesh]] in the [[Static Mesh Editor]], in Details panel > LOD Picker > LOD.
The **transition point**, when to switch between LOD levels, is set by selecting a LOD level with the LOD Picker and then editing **Screen Size** in Details panel > LOD # (the LOD level picked) > Screen Size.
Not sure if this is the upper or lower bound, i.e. if this is the point when it switches to this LOD when moving towards or away from the mesh.
LOD levels with a higher number in their name should have a smaller Screen Size value than LOD levels with a smaller number, you can think of the number as a distance from the [[Camera]].
Small number means short distance means large screen size means many triangles.
Large number means long distance means small screen size means few triangles.
The source mesh is LOD 0.
Screen Size will be write-protected if Details panel > LOD Settings > Auto Compute LOD Distances is checked.
We can set Details > LOD Settings > Minimum LOD to limit how detailed LOD version a particular [[Static Mesh Asset]] will use.
This is useful to improve performance at the cost of some geometry quality.

We can force a particular [[Static Mesh Component]] to use a particular LOD level with Details panel > LOD > Forced LOD Model.

There is a [[View Mode]] named Level Of Detail Coloration > Mesh LOD Coloration, that shows the currently selected LOD level for each [[Static Mesh]] in the level.


# LOD Preview

The [[Static Mesh]] is shown in a viewport in the [[Static Mesh Editor]].
The current LOD level and some statistics about it is shown in the upper-left corner of the viewport.
Moving the camera back and forth, by holding the right mouse button (I think.), will let you see at which size the switch between LOD levels happen and what the transition looks like.
To see a fixed LOD level regardless of screen size select the LOD level to see in Details panel > LOD Picker > LOD.
The selected LOD level will be shown in the viewport regardless of camera position.
To set it back to automatic switching select LOD Auto.
This setting does not affect regular rendering in the Level Viewport or a packaged game.

# LOD Generation

All of these settings are in [[Static Mesh Editor]] > Details panel.

## Automatic LOD generation - LOD Group

Set in LOD Settings > LOD Group.
Select the group that best matches the type and usage of your [[Static Mesh]].
It will set some (or all?) of the settings in the LOD Settings category in the Details panel to a preset for that LOD group.
A number of LOD levels will be generated from the source mesh, which becomes LOD 0.

## Automatic LOD Generation - Manual Settings

To have more control over the automatic LOG generation set LOD Settings > LOD Group to None.
Set LOD Settings > Number Of LODs to the number of LOD levels you want to have, including LOD 0 for the source mesh, so LOD 0 to LOD #-1.
Disable LOD Settings > Auto Compute LOD Distances.
Click LOD Settings > Apply Changes.
Unreal will now generate the LOD levels for us.

There are a few per-LOD level settings that we can tweak.
Enable LOD Picker > Custom and select all LOD# check boxes that appear.
This will display the per-LOD level settings for all levels as separate categories.
These settings include a Screen Size property as a fractions of the entire screen.
Not sure if area, horizontal, or vertical size.
You can also control the amount of triangles generated for each LOD level in the LOD # > Reduction Settings property group.

Click LOD Settings > Apply Changes to generate the new LOD levels.

# Distance Culling

The most extreme form of LOD is to not render a mesh at all.
This can be done for a [[Static Mesh Component]] with Details panel > LOD > **Desired Max Draw Distance**.
When the mesh if further from the camera then this then it won't be rendered.
It's "desired" because the distance can be set by other things as well, such as a Cull Distance Volume (I think.).
A mesh can also be prevented from rendering by setting Details panel > LOD > **Detail Mode**.
The Detail Mode, one of Low, Medium, High, control at what details settings this mesh should start to be included in.
If detail mode >= system detail mode, then the mesh won't be rendered.
Set small clutter to High, meaning that they are rendered only on High settings, set the important bits to Low, meaning that they are rendered on Low and up.
```
    m e s h
    l  m  h
s l ✓  x  x
y m ✓  ✓  x
s h ✓  ✓  ✓
```



# References

- [_5 Tips to Optimize Environments in Unreal Engine 4_ - LOD by Jakub Haluszczak @ youtube.com 2021](https://youtu.be/gZkKcaF4Ifk?t=150)