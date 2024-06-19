Level Streaming is a way to break our world up into multiple [[Level]]s and pick-and-chose which parts should be included or not at any point in time.

There is always one main [[Level]] that is the parent [[Level]], the [[Level]] that we start from.
This is called the Persistent [[Level]].
Additional [[Level]]s can be loaded in.
The additional [[Level]]s are called sublevels.

Streaming [[Level]]s are controlled from the Levels panel, opened with [[Top Menu Bar]] > Window > Levels.

[[Level]] streaming is controlled per sublevel and can be either
- Blueprint: Game logic control when a particular level should be loaded or unloaded.
- Always Loaded: The sublevel structure is used for organization within the Unreal Editor only. Once in-game the sublevel will always be loaded.

Change between Blueprint and Always Loaded with Levels panel > sublevel > right-click > Change Streaming Method.


# Levels Panel

The Levels panel is opened with [[Top Menu Bar]] > Window > Levels.
The main part of the Levels panel is a tree of [[Level]]s rooted at the Persistent Level, the [[Level]] [[Asset]] that is opened in the [[Level Viewport]].
Clicking the eye icon in the far left hides all [[Actor]]s from the sublevel.

Change between Always Loaded and Blueprint-controlled streaming with Levels panel > sublevel > right-click > Change Streaming Method.


# Adding A Streaming Level

An entire [[Level]] can be added as a sublevel to the current Persistent Level by dragging it from the [[Content Browser]] to the Levels panel.
The sublevel is created with Streaming Method set to Blueprint so if you want the [[Actor]]s in the sublevel to show up as-if they were part of the Persistent Level then right-click the sublevel in the Levels panel and select Change Streaming Method > Always Loaded.


# Creating A New Streaming Level

## From Actors In Level

We can take a set of [[Actor]]s in the current [[Level]] and move them to a new Streaming Level.
Select the [[Actor]]s to be moved in the [[Outliner]] or [[Level Viewport]].
Open [[Top Menu Bar]] > Window > Levels.
Top left corner > Levels drop-down > Create New With Selected Actors....
Give the new [[Level]] a location within the Content folder and a name.
The new level will be added as a sublevel of the Persistent Level.


# References

- [_Introducing Post Processing_ by Epic  Online Learning, Kevin Lyle @ dev.epicgames.com/courses 2023 UE5.0](https://dev.epicgames.com/community/learning/courses/pE2/unreal-engine-introducing-post-processing/mZ11/unreal-engine-introducing-post-processing-overview)
