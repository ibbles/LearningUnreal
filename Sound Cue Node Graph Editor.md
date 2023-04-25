This note outlines how the [[Sound Cue Editor]] has been implemented.
I'm not allowed to show much of the Unreal Engine code due to licensing restrictions,
but I can at least given an overview of the class relationships.
This analysis was done on Unreal Engine 5.1.1.
See also [[Custom Node Graph Editor]] for a description of the node graph editor system.
See also [[Node Graph Editor]] for an overview of the engine classes that make up the node graph editor toolkit.

Here is a screenshot of the [[Sound Cue Editor]], where a sound cue [[Asset]] is being edited.
![[Images/Sound_cue_graph.png]]

We see three tabs:
- Details panel
- Viewport
- Palette

The main focus of this note is the Viewport.


The implementation consists of the following header files:
- `SoundCue.h`
	- Contains `USoundCue`, which is an `UObject`. Is an [[Asset]].
	- Contains `ISoundCueAudioEditor`, an interface class.
- `SoundCueFactoryNew.h`
	- Contains `USoundCueFactoryNew`, which is an `UFactory`.
- `SoundCueGraph.h`
	- Contains `USoundCueGraph`, which is an `UEdGraph`.
- `SoundCueGraphNode_Base.h`
	- Contains `USoundCueGraphNode_Base`, which is an `UEdGraphNode`.
- `SoundCueGraphNode.h`
	- Contains `USoundCueGraphNode`, which is an `USoundCueGraphNode_Base`.
- `SoundCueGraphSchema.h`
	- Contains a bunch of  subclasses of `FEdGraphSchemaAction`.
	- Contains `USoundCueGraphSchema`, which is an `UEdGraphSchema`.
- `SoundCueGraphNode_Root.h`
	- Contains `USoundCueGraphNode_Root`, which is an `USoundCueGraphNode_Base`.
- `SoundCueEditor.h`
	- Contains `FSoundCueEditor`, which implements `ISoundCueEditor`.
- `SoundCueGraphNodeFactory.h`
	- Contains `FSoundCueGraphNodeFactory`, which is a `FGraphPanelNodeFactory`.
- `SoundCueGraphEditorCommands.h`
	- Contains `FSoundCueGraphEditorCommands`, which is a `TCommands`.
- `SoundCueEditorUtilities.h`
	- Contains `FSoundCueEditorUtilities`, which is a bunch of functions.
- `SoundNode.h`
	- Contains `USoundNode`, which is an `UObject`.
- `SoundCueGraphConnectionDrawingPolicy.h`
	- Contains colors and wire thickness data.


# Classes

## `USoundCue`

Defined in `SoundCue.h`.
A Runtime class.
Inherits from `UObject`.
Is an [[Asset]], meaning that is has a corresponding `UFactory` and `FAssetTypeActions_Base`.
Contains a list of `USoundNode`.
Has a bunch of [[Property|Properties]] that show up in the [[Details Panel]] in the sound cue editor.
Some of the properties are within `#if WITH_EDITORONLY_DATA` guards and excluded from the [[Details Panel]] by not having a [[Property Specifier]].

Has an `UEdGraph` member variable named `SoundCueGraph`.
Always points to a `USoundCueGraph`.
Is initialized in `PostInitProperties`.
Creation goes via `ISoundCueAudioEditor`.
`ISoundCueAudioEditor` is a singleton class, static member variable.
The instance is created by `USoundCueGraph`  once.

Has function `ConstructSoundNode`.
Has a bunch of `WITH_EDITOR` functions dealing with the `UEdGraph` and `ISoundCueAudioEditor`.


## `USoundCueFactoryNew`

Defined in `SoundCueFactoryNew.h`.
An Editor class.
Inherits from `UFactory`.
Create new `USoundCue` instances using `NewObject<USoundCue>`.


## `FAssetTypeActions_SoundCue`

Defined in `AssetTypeActions_SoundCue`.
Is an Editor class.
Inherits from `FAssetTypeActions_Base`.

Opens the asset editor for a`USoundCue`.
Does this via `IAudioEditorModule`, by calling `IAudioEditorModule::CreateSoundCueEditor`.
Allocates a new `FSoundCueEditor`.


## `USoundNode`

Defined in `SoundNode.h`.
A Runtime class.
Inherits from `UObject`.
Has a list of `USoundNode` children.
Has an `UEdGraphNode`, which only points to `USoundCueGraphNode`. (I assume.)
Declares a bunch of virtual functions.

The sound effects that can be added to a `USoundCue` are all subclasses of `USoundNode`.
For example `USoundNodeDoppler` and `USoundNodeDelay`.


## `ISoundCueAudioEditor`

Defined in `SoundCue.h`.
A Runtime class.
Contains a bunch of pure-virtual functions for manipulating sound and graph nodes.


## `USoundCueGraph`

Defined in `SoundCueGraph.h`.
An Editor class.
Inherits from `UEdGraph`.
A very small class.
Overrides no virtual functions.
Has a single function: `USoundCue* GetSoundCue()`.
Returns the `USoundCue` this graph has been created for.

Hidden in the `.cpp` file is `FSoundCueAudioEditor`, which inherits from `ISoundCueAudioEditor`.
That is the interface class defined in `SoundCue.h`.
`FSoundCueAudioEditor` is a singleton, created by the first `USoundCueGraph` constructor call.
`USoundCue` asks `FSoundCueAudioEditor`, the singleton, to create a `USoundCueGraph` for it in `PostInitProperties`.
So every `USoundCue` has a `USoundCueGraph` at all times, ready to be shown in a `FSoundCueEditor`.


## `USoundCueGraphNode_Base`

Defined in `SoundCueGraphNode_Base.h`.
An Editor class.
Inherits from `UEdGraphNode`.
Has a bunch of pin-related functions.


## `USoundCueGraphNode`

Defined in `SoundCueGraphNode.h`.
An Editor class.
Inherits from `USoundCueGraphNode_Base`.
Has a pointer to `USoundNode`.
Has a bunch of pin-related functions.


## `USoundCueGraphNode_Root`

Defined in `SoundCueGraphNode_Root.h`.
An Editor class.
Inherits from `USoundCueGraphNode_Base`.
I think this is the output node.


## `USoundCueGraphSchema`

Defined in `SoundCueGraphSchema.h`.
An Editor class.
Inherits from `UEdGraphSchema`.
Has a bunch of link related functions.
Manages a bunch of actions, subclasses of `FEdGraphSchemaAction`.


## `FSoundCueEditor`

Defined in `SoundCueEditor.h`.
An Editor class.
Inherits from `ISoundCueEditor` and a few utility classes.
Has a bunch of node related functions.
Has a `USoundCue`, `SGraphEditor`, and tabs with Details panel and Palette.
This is the main editor window for a sound cue asset.
Is created by `FAssetTypeActions_SoundCue::OpenAssetEditor`,
with some help from `IAudioEditorModule::CreateSoundCueEditor`.


## `FSoundCueGraphNodeFactory`

Defined in `SoundCueGraphNodeFactory.h`.
An Editor class.
Inherits from `FGraphPanelNodeFactory`.
Creates a `SGraphNode` from a `UEdGraphNode`.
Always creates a `SGraphNodeSoundBase` for all types of `USoundCueGraphNode`.
`USoundCueGraphNode_Root` get a `SGraphNodeSoundResult`.


## `FSoundCueGraphEditorCommands`

Defined in `SoundCueGraphEditorCommands.h`.
An Editor class.
Inherits from `TCommands`.
Contains a bunch of `FUICommandInfo` for a number of things that the user can do.


## `FSoundCueEditorUtilities`

Defined in `SoundCueEditorUtilities.h`.
An Editor class.
Various helper functions.
