# Unreal Editor

The Unreal Editor can render itself at various scales in order to support monitors with different sizes and resolutions, resulting in different DPIs.
Sometimes it gets confused and pick a very inappropriate size.
The scaling guessing can be disabled with [[Editor Preferences]] > General > Appearance > User Interface > Enable High DPI Support.
A scaling slider would be helpful, but does not exist in the [[Editor Preferences]] as of Unreal Engine 5.3.
There is a plugin, [Editor UI Scale @ unrealengine.com/marketplace](https://www.unrealengine.com/marketplace/en-US/product/editor-ui-scale) that adds a [[Editor Preferences]] setting.
There is a temporary scale slider at Top Menu Bar > Tools > Debug > Widget Reflector > top left corner.
This needs to be re-applied every time the editor is started.
I sometimes misremember the name as Widget Inspector.

# Game UI

The display DPI influences what the game UI, e.g. [[Unreal Motion Graphics - UMG]], will look like.

# References

- [_DPI Scaling_ @ dev.epicgames.com/documentation ](https://dev.epicgames.com/documentation/en-us/unreal-engine/dpi-scaling-in-unreal-engine)
