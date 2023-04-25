This notes contains summaries of references I have found dealing with the creation of custom node graph editors.
That is, creating new asset types that store a graph / network data and an editor for those assets.
This could be a pipe network, rooms connected with portals, or a factory with machines connected by a conveyor system.

For copy-paste:

Author: 
Date: 
Format:
Platform: 
URL: 


# Custom Node / Graph Editor

Author: OuchImBad
Date: 2021-04-08
Format: Forum thread
Platform: Unreal Engine Forums
URL: https://forums.unrealengine.com/t/custom-node-graph-editor/212058

OuchImBad asks how to create a node graph editor like Blueprints and Materials.
A reply lists a bunch of documentation pages:
- [Toolkits | Unreal Engine Documentation 110](https://docs.unrealengine.com/en-US/API/Editor/UnrealEd/Toolkits/index.html)
- [UEdGraph | Unreal Engine Documentation 70](https://docs.unrealengine.com/en-US/API/Runtime/Engine/EdGraph/UEdGraph/index.html)
- [UEdGraphNode | Unreal Engine Documentation 41](https://docs.unrealengine.com/en-US/API/Runtime/Engine/EdGraph/UEdGraphNode/index.html)
- [SGraphEditor | Unreal Engine Documentation 51](https://docs.unrealengine.com/en-US/API/Editor/UnrealEd/SGraphEditor/index.html)
- [SGraphNode | Unreal Engine Documentation 27](https://docs.unrealengine.com/en-US/API/Editor/GraphEditor/SGraphNode/index.html)
- [UEdGraphSchema\_K2 | Unreal Engine Documentation 20](https://docs.unrealengine.com/en-US/API/Editor/BlueprintGraph/UEdGraphSchema_K2/index.html)
- [FEdGraphUtilities | Unreal Engine Documentation 26](https://docs.unrealengine.com/en-US/API/Editor/UnrealEd/FEdGraphUtilities/index.html)

The Sound Graph is suggested as a starting point.
By this I assume they mean to study Unreal Engine source code.


# [C++ Plugin] Create Unreal-like Graph

Author: MRCookie
Date: 2016-07-19
Format: Forum thread
Platform: Unreal Engine Forums
URL: https://forums.unrealengine.com/t/c-plugin-create-unreal-like-graph/364515

This is a fairly old thread, so not sure how much of the information is still relevant.
Foundational stuff like this usually don't change all that much, so I hope it's still good.

The OP want to create a graph editor.
Not a lot of detail as to what kind of editor.

anonymous\_user\_f5a50610 replies on 2016-06-20 with an outline of what is needed:
- Two parts
	- Node data + editor.
	- Runtime code to act on node data.
- Node data does nothing without runtime code.
- Runtime state is created from node data.
- Best way to learn is to mimic a simple editor.
- Suggests looking into sound cue and sound class editors.
- Node graph editor has two parts
	- Editor that opens when double-clicking an asset.
	- Graph widget.
- Engine code typically separate the two in source code.
- Graph
	- Source code in a `Private` directory.
	- Make a class that inherits from `FAssetEditorToolkit`.
	- Contains widgets.
- Widget
	- Make a class that inherits from `UEdGraph` and others.
	- Has a `Schema` class that describe graph behavior, creation of nodes, allow connections, and such.
- A `Node` class describe what the graph can contain.
	- Should not contain any logic, just a description and topology.
	- Can have a single `Node` class that describe many different types of things.
- Implement the runtime part first, without any editor code.
- Then create the editor and a way to create the runtime nodes from the editor nodes.
- Look at engine code.

This was a lot of good information.
Another recommendation to look at the Sound Cue editor, literally `SoundCueEditor`.

- Use the reflection system to execute functions from the graph editor.
- Can find all `UFunction` objects somehow, filter by Blueprint Callable, execute with [`ProcessEvent`](https://docs.unrealengine.com/5.1/en-US/API/Runtime/CoreUObject/UObject/UObject/ProcessEvent).
	- The parameter to `ProcessEvent` is a struct having members that match the parameter list of the called function.
	- Order and type; not sure if name must match as well.
- There is also [`Invoke`](https://docs.unrealengine.com/5.1/en-US/API/Runtime/CoreUObject/UObject/UFunction/Invoke/).
	- Don't know much about that.


We will need a way to create the new asset type, see [_Creating a Content Browser Asset in UE4_ by Matt Stafford @ wraiyth.com](https://www.wraiyth.com/?p=209).
From 2015, so also somewhat old.

An editor is associated with a particular asset type in the subclass of `FAssetTypeActions`.
```cpp
void FAssetTypeActions_YourAssetTypeClass::OpenAssetEditor(
	const TArray<UObject*>& InObjects,
	TSharedPtr<IToolkitHost> EditWithinLevelEditor);
```

`FAssetTypeActions` are registered with [`IAssetTools::RegisterAssetTypeActions`](https://docs.unrealengine.com/4.27/en-US/API/Developer/AssetTools/IAssetTools/RegisterAssetTypeActions/).

To do graph-stuff your Editor module need `GraphEditor` in `PrivateDependencyModuleName`.

`GraphEditor.h` is a relevant file.

Some example code for creating 
```cpp
// Not sure which function this code should go into.
TSharedRef<SDockTab> TAB = SNew(SDockTab).TabRole(ETabRole::NomadTab);
TAB->SetContent(SNew(UMyCustomGraphWidget)); // UMyCustomGraphWidget is our class, inheriting from SCompoundWidget.

// Construct:
ParentGraphEditor = SNew(SGraphEditor).GraphToEdit(UMyEdGraph);
ChildSlot
[
	SNew(SVerticalBox)
	+ SVerticalBox::Slot()
	.FillHeight(1.f)
	[
		SNew(SOverlay)
		+ SOverlay::Slot()
		[
			ParentGraphEditor.ToSharedRef()
		]
	]
];
```

There are too many details missing here for me to understand what to do.


# DialoguePluginSystem

Author: NotYetGames, Daniel Butum
Format: Unreal Engine plugin
Platform: GitLab
URL: https://gitlab.com/NotYetGames/DlgSystem

Not sure how much additional usefulness this has over the built-in graph editors for learning.
The relevant bits are in [`Source/DlgSystemEditor/Editor`](https://gitlab.com/NotYetGames/DlgSystem/-/tree/master/Source/DlgSystemEditor/Editor).


# UE4 Asset Editor Graph View

Author: easycomplex-tech
Date: Not provided. Unreal Engine 4 time-frame.
Format: Blog post
Platform: Self-hosted
URL: https://easycomplex-tech.com/blog/Unreal/AssetEditor/UEAssetEditorDev-AssetEditorGraph/

This post is part of a series of [Asset Editor @ easycomplex-tech.com](https://easycomplex-tech.com/blog/Unreal-AssetEditor/) posts.

Need to derive from two classes:
- `UEdGraphSchema`: Actions the user can perform on the graph.
- `UEdGraph`: The graph data.

Provides a class definition of a `UEdGraphSchema` subclass, listing the virtual functions.
And a class definition of a `UEdGraph` subclass as well, but mostly empty.

`SGraphEditor` is a widget designed to hold/display a `UEdGraph`.

`FDocumentTabFactoryForObjects` and `FDocumentTracker` is used to manage a tab to hold the `SGraphEditor`.
I think... 
There is also `FTestGraphEditorSummoner`.

I didn't understand much of this one.


# Heart Graph Plugin

Author: Drakynfly
Date: Active as of 2023-04-18
Format: Unreal Engine plugin
Platform: GitHub
URL: https://github.com/Drakynfly/HeartGraph

Another plugin that contains a node graph editor.
Also has a runtime editor.


# GenericGraphPlugin

Author: Jinyu Liao
Date: As of 2023-04-20 last commit was on 2022-06-12.
Format: Unreal Engine plugin
Platform: GitHub
URL: https://github.com/jinyuliao/GenericGraph

Another plugin, one that describes itself as being reusable.

