Data Layers provide granular level of control over asset streaming.
A way to have sets of [[Actor]]s in the [[Level]] that are enabled, i.e. brought in, on demand based on game logic decisions.

A Data Layer contains a list of referenced [[Actor]]s.
An [[Actor]] can be referenced by multiple Data Layers.
The [[Actor]] is enabled if any, some, or all of those layers are enabled.
An [[Actor]] is only disabled if all Data Layers it is part of  are disabled.

# Use Cases

Data Layers can be used to
- add mission-specific [[Actor]]s to the world.
- alter the world.
	- For example a change in season or moving forward in time.
- completely replace the world.
- bring in building interiors
- do traversal streaming.
- control performance scaling.
	- Simply don't ever load some layers that aren't necessary for the game to work on weaker platforms or lower graphical settings levels.
- organize [[Actor]]s when working in the Unreal Editor.
	- A Data Layer can be either a Runtime Data Layer or an Editor Data Layer.
	- Not sure what it means for a Data Layer to be an Editor Data Layer.
	- Are the [[Actor]]s in the Data Layer simply inserted into the [[Level]] during project packaging?


# Data Layers Panel

Data Layers are managed in the Data Layers panel, opened with [[Top Menu Bar]] > Window > (World Partition > ?) Data Layers (Outliner?).
Create a new Data Layer with Data Layers panel > right-click > Create (Empty|Private) Data Layer.
Use the eye button on the Data Layer's row to hide or unhide all [[Actor]]s part of that Data Layer in the [[Level Viewport]].
Note than an [[Actor]] in the Data Layer may remain visible if it is also assigned to another Data Layer that is visible.

A Data Layer can have Is Dynamically Loaded enabled:

> Whether the Data Layer affects actor runtime loading.

(
Not sure what the behavior is when Is Dynamically Loaded is not enabled.
Are all [[Actor]]s in the Data Layer always loaded?
Are the [[Actor]]s in the Data Layer loaded as-if they weren't part of any Data Layer, i.e. by the regular World Partition cell streaming?
)

When Is Dynamically Loaded is enabled we get additional Data Layer settings.
- Initial State.
	- Unloaded: The Data Layer must be explicitly loaded using gameplay logic. See _Data Layer API_ below.
	- Loaded: The Data Layer is loaded automatically, but not yet activated. Meaning they don't yet exist in the world but can be activated immediately.
		- (Not sure what this means. Are all [[Actor]]s in the Data Layer loaded together, or are they loaded using the regular cell-based streaming?)
	- Activated: The content is loaded and activated as soon as it is ready.

An [[Actor]] is assigned to a Data Layer with [[Details Panel]] > Data Layers > Data Layers.
An [[Actor]] can be assigned to multiple Data Layers.
One or more [[Actor]]s can be assigned to one or more Data Layers by selecting the [[Actor]]s in the [[Level Viewport]] and then do Data Layers panel > a Data Layer > right-click > Add Selected Actors To Selected Data Layers.
This will modify the Data Layers property of all those [[Actor]]s.

A Data Layer can be made the current Data Layer.
When there is a current Data Layer then any new [[Actor]] that is created will be added to that Data Layer.
(TODO Figure out how to make a Data Layer current.)

# Data Layer API

The type of a Data Layer, e.g. when creating a [[Blueprint Variable]], is Actor Data Layer.

Runtime Data Layer control is done using the Data Layer Subsystem.
Available in Blueprint with the Get Data Layer Subsystem node.
A Data Layer's state, i.e. loaded or unloaded, is controlled with Set Data Layer State.
Needs to know which Data Layer to change the state of.
This can be done by creating a [[Blueprint Variable]] of type Actor Data Layer.
The Default Value for an Actor Data Layer variable is a drop-down list of all Data Layers created in the Data Layers panel.

Bringing in a Data Layer can be done in two steps.
The first step is to set the Data Layer State to Loaded.
This can be done some time in advance since the [[Actor]]s in the Data Layer won't be added to the world immediately.
Once the game state is such that the [[Actor]]s in the Data Layer should appear then the Data Layer State is set to Activated.

To remove all the [[Actor]]s in a Data Level from the world set the Data Layer state to Unloaded.

Call Flush Level Streaming to block until the currently ongoing streaming operations have finished.
(
I think.
)

# Data Layer Console Commands

- `wp.DumpDataLayers`
- `wp.Runtime.DebugFilterByDataLayer`
- `wp.Runtime.SetDataLayerRuntimeState STATE LAYER`
- `wp.Runtime.ToggleDataLayerActivation LAYER`
- `Runtime.ToggleDrawDataLayers`


# References

- [_Large worlds in UE5: A whole new (open) world | Unreal Engine_ by Epic Online Learning 2021 UE5.0](https://dev.epicgames.com/community/learning/talks-and-demos/KBe/large-worlds-in-ue5-a-whole-new-open-world-unreal-engine)
- [_Building Open Worlds in Unreal Engine 5 | Unreal Fest 2022_ by Epic Online Learning @ dev.epicgames.com/talks-and-demos](https://dev.epicgames.com/community/learning/talks-and-demos/LLM5/building-open-worlds-in-unreal-engine-5-unreal-fest-2022)
- [_Building Bigger: Changing Your Workflow for Building Worlds instead of Scenes | Unreal Fest 2023_ by Chris Murphy @ dev.epicgames.com/talks-and-demos 2024 UE5.3](https://dev.epicgames.com/community/learning/talks-and-demos/jwlJ/unreal-engine-building-bigger-changing-your-workflow-for-building-worlds-instead-of-scenes-unreal-fest-2023)


