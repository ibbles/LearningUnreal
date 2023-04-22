This note outlines how the [[Sound Cue Editor]] has been implemented.
I'm not allowed to show much of the Unreal Engine code due to licensing restrictions,
but I can at least given an overview of the class relationships.
This analysis was done on Unreal Engine 5.1.1.
See also [[Custom Node Graph Editor]] for a description of the node graph editor system.
See also [[Node Graph Editor]] for an overview of the engine classes that make up the node graph editor toolkit.

Here is a screenshot of the sound cue editor, where a sound cue [[Asset]] is being edited.
![[Images/Sound_cue_graph.png]]


The implementation consists of the following header files:

- `SoundCue.h`
	- Contains `USoundCue`, which is an `UObject`. Is an [[Asset]].
	- Contains `ISoundCueAudioEditor`, an interface class.
- `SoundCueFactoryNew.h`
	- Contains `USoundCueFactoryNew`, which is an `UFactory`.
- `SoundCueGraph.h`
	- Contains `USoundCueGraph`, which is an `UEdGraph`.
- `SoundCueGraphNode_Base.h`
	- Contains `USoundCueGraphNode_Base`.
- `SoundCueGraphNode.h`
	- Contains `USoundCueGraphNode`.
- `SoundCueGraphSchema.h`
	- Contains a bunch of  subclasses of `FEdGraphSchemaAction`.
	- Contains `USoundCueGraphSchema`.
- `SoundCueGraphNode_Root.h`
	- Contains `USoundCueGraphNode_Root`.
- `SoundCueEditor.h`
	- Contains `FSoundCueEditor`.
- `SoundCueGraphNodeFactory.h`
	- Contains `FSoundCueGraphNoeFactory`.
- `SoundCueGraphEditorCommands.h`
	- Contains `FSoundCueGraphEditorCommands`.
- `SoundCueEditorUtilities.h`
	- Contains `FSoundCueEditorUtilities`.
- `SoundNode.h`
	- Contains `USoundNode`.
- `SoundCueGraphConnectionDrawingPolicy.h`
	- Contains colors and wire thickness.


# Classes

## `USoundCue`

Defined in `SoundCue.h`.
A Runtime class.
Inherits from `UObject`.
Is an [[Asset]].
Contains a list of `USoundNode`.
Has a bunch of [[Property|Properties]] that show up in the [[Details Panel]] in the sound cue editor.
Some of the properties are within `#if WITH_EDITORONLY_DATA` guards and excluded from the [[Details Panel]] by not having a [[Property Specifier]].
One of those properties is a `UEdGraph SoundCueGraph`, which is initialized in `PostInitProperties`.
Has function `ConstructSoundNode`.
Has a bunch of `WITH_EDITOR` functions dealing with the `UEdGraph` and `ISoundCueAudioEditor`.


## `USoundCueFactoryNew`

Defined in `SoundCueFactoryNew.h`.
An Editor class.
Inherits from `UFactory`.
Create new `USoundCue` instances using `NewObject<USoundCue>`.


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


## `USoundCueGraphSchema`

Defined in `SoundCueGraphSchema`.
An Editor class.
Inherits from `UEdGraphSchema`.
Has a bunch of link related functions.
Manages a bunch of actions, subclasses of `FEdGraphSchemaAction`.


## `USoundCueGraphNode_Root`

Defined in `SoundCueGraphNode_Root.h`.
An Editor class.
Inherits from `USoundCueGraphNode_Base`.
I think this is the output node.


## `FSoundCueEditor`

Defined in `SoundCueEditor.h`.
An Editor class.
Inherits from `ISoundCueEditor` and a few utility classes.
Has a bunch of node related functions.
Has a `USoundCue`, `SGraphEditor`, and some tabs.


## `FSoundCueGraphNoeFactory`

Defined in `SoundCueGraphNodeFactory.h`.
An Editor class.
Inherits from `FGraphPanelNodeFactory`.
Creates a `SGraphNode` from a `UEdGraphNode`.


## `FSoundCueGraphEditorCommands`

Defined in `SoundCueGraphEditorCommands.h`.
An Editor class.
Contains a bunch of `FUICommandInfo` for a number of things that the user can do.


## `FSoundCueEditorUtilities`

Defined in `SoundCueEditorUtilities.h`.
An Editor class.
Various helper functions.


## `USoundNode`

Defined in `SoundNode.h`.
A Runtime class.
Inherits from `UObject`.
Has a list of `USoundNode` children.
Has an `UEdGraphNode`.
Declares a bunch of virtual functions.
