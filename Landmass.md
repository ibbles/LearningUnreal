Landmass was introduce as a beta feature in 4.26.
It is a plugin that uses splines and such to non-destructively sculpt a [[Landscape]].
By non-destructive we mean that the Landmass tools don't directly modify the Landscape heights.
To enable the plugin do Top Menu Bar > Edit > Plugins > Search `Landmass` > Tick Enabled.

# Edit Layer

Edit layers are like the layers in an image editing software.
The layers are listed in the Sculpt tab of the Landscape panel.
Each layer contribute height offsets to the layers beneath it.
The final Landscape shape is the combination of all the layers.
An Edit Layer can contain Blueprint Brushes.
The Blueprint Brushes in an Edit Layer is listed in the Edit Layer Blueprint Brushes category of the Landscape panel when an Edit Layer is selected in the Landscape panel.

To use Landmass with a Landscape tick the Enable Edit Layers in the New Landscape panel.
You can check if an existing Landscape has Edit Layers enabled from the [[Landscape Mode]] > Landscape panel.
To create a new layer right-click an existing layer and select Create.
You can name the new layer using right-click > Rename.


# Blueprint Brush

A Blueprint Brush  is a type of Blueprint that modifies the heights of a Landscape.
A Blueprint Brush is part of Edit Layer.

To add a Blueprint Brush to an Editor Layer:
- Select Landscape panel > Sculpt > Blueprint.
	- If you don't see the Blueprint tool next to Scrupt, Erase, Smooth, Flatten, etc then the Landmass plugin hasn't been enabled.
- Select a type of Blueprint Brush to add in Landscape panel > Tool Settings > Blueprint Brush.
	- `CustomBrush_Landmass` is useful for mountains.
- Select the Edit Layer to add the Blueprint Brush to in Landscape panel > Edit Layers.
- Click in the Viewport to create the Blueprint Brush instance.

The Blueprint Brush instance is an Actor that contains a Spline.
The Spline is used if Blueprint Brush instance > Details panel > Brush Settings Brush Type is set to Landmass Outline or 



# References

- [_Advanced Skill Sets for Environment Art_ > _Landscape Sculpting and Landmass_ by Epic Online Learning 2022 UE4.27](https://dev.epicgames.com/community/learning/courses/Qwa/unreal-engine-advanced-skill-sets-for-environment-art/0Rz6/landscape-sculpting-and-landmass)
