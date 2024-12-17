A Level is a scene of sorts, a stage where we can put [[Actor|Actors]].
It is where everything made for the game comes together.
Closely related to [[Map]].
The difference between a Level and a [[Map]] is unclear to me.

Level is an [[Asset]] which means that they live in the [[Content Browser]].
The level and all its contents, i.e. the Actors, can be stored in a single `.umap` file.
The level and it's contents can also be stored separate from each other by enabling [[One File Per Actor]].
A level may also be split up into multiple chunks using [[World Partition]].
Whether or not a level uses [[World Partition]] is selected when the level is created, or with Top Menu Bar > Tools > Convert Level.
[[World Partition]] controls streaming.

Create a new Level by right-click in the [[Content Browser]] and select Level.

An [[Actor]] placed in the level, and any [[Actor Component]] it contains, is called an instance and the properties of that particular instance can be modified from the [[Level Editor]] [[Details Panel]] when the [[Actor]] or [[Component]] is selected.

The main Unreal Editor windows is actually the [[Level Editor]] and the window showing the scene is the [[Level Viewport]].
When you open a Level by double-clicking it in the [[Content Browser]] then the Level currently open is closed and the double-clicked one is opened.

An [[Actor]] can be added to the level using [[Main Tool Bar]] > Place Actor > drag an Actor type into the [[Level Viewport]] where you want it.
The Place Actor menu is searchable, meaning you can type the name of the Actor type you want to filter the list.
Each Actor type may be part of multiple categories and may therefore appear multiple times in the list.

An Actor can be added to the level by dragging an [[Asset]] from the [[Content Browser]] onto the [[Level Viewport]].

A level may require building before it can be used, depending on what features the level uses.
The following features require building:
- [[Baked Lighting]]
- Some properties of [[Landscape]].
- [[HLOD]].

You can set the Level that is opened when you open your project in Unreal Editor and the Level that the game starts at in the [[Project Settings]], under Project > Maps & Modes > {Editor Startup Map, Game Default Map}.


# Level Transition Open Level Load Level

To end one level and transition to that level, i.e. to load a level, during runtime call the Open Level [[Blueprint Function]].

# Sub-Levels

Sub-levels are used to let different people work with different parts of the Level at the same time.
This is difficult with a single `.umap` file since it is difficult to merge changes made by different people on different machines.
With sub-Levels the [[Actor|Actors]] in the Level are split over multiple `.umap` files.
As long as people work on different `.umap` files there is no issue with editing at the same time.
The [[Actor|Actors]] does not have to be split based on where in the Level they are.
They can also be split based on [[Actor]] type, gameplay logic relation, role or responsibility, or any other division you see fit.
In this sense, a Level is just a collection of [[Actor|Actors]].

Sub-Levels can also be an optimization or game-play technique.
By loading and unloading sub-Levels, called [[Level Streaming]], it is possible to control what exists in the game at runtime.
[[World Partition]] takes over some of the streaming responsibility.

When working with sub-levels, be very conscious about which Level is the currently active Level,
so that you make changes where you intend to.
Sub-Levels are controlled from the Levels panel.
You can ensure you don't accidentally edit a Level you don't intend to edit by locking it in the Levels panel.
Actors from a locked Level will be gray in the [[Outliner]].

The Levels panel is opened from [[Top Menu Bar]] > Window > Levels.
The currently active Level is highlighted in blue.
Make a Level current by right-clicking it in the Level panel and select Make Current.
Make the Level that holds a particular [[Actor]] the current level by selecting the [[Actor]] and right-click > Level > Make Current Level: `LEVEL_NAME`.
`LEVEL_NAME` will be the name of the Level that the [[Actor]] is part of.

The main levels are called persistent levels.
There are also sub-levels.
A sub-level is a collection of [[Actor|Actors]] that can be streamed in and out on demand.
See [[Level Streaming]].
A sub-level can be made to always be loaded with Levels panel > level > right-click > Change Streaming Method > Always Loaded.

A new Level is created from the Levels drop-down in the top-left corner of the Levels panel > Create New or Create New With Selected Actors.
Create New With Selected Actors will move the selected [[Actor|Actors]] from the Level they are currently in to the new Level.
A window opens to let you select where the new Level should be saved and what it should be named.
The newly created level will become the currently active level.
The newly created level will be made a child of the persistent level.

An already existing level can be added as a sub-level to the current persistent level.
Levels panel > Levels drop-down > Add Existing.
A window opens showing Level assets in the [[Content Browser]].

A Level can be removed from the persistent Level with Levels panel > the level to remove > right-click > Remove Selected.
This will remove the Level, and all [[Actor|Actors]] it contains, from the persistent Level.
The Level [[Asset]] will sill be kept in the [[Content Browser]].

Adding a sub-level to a persistent level will cause all the Actors in the sub-level to appear in the [[Level Viewport]].
By clicking the eye next to the sub-level in the Levels panel all Actors from that sub-level will be hidden in the [[Level Viewport]].

You can find which Level [[Asset]] a particular [[Actor]] instance comes from by selecting the [[Actor]] and right-click > Level > Find Actor Level In Content Browser.

Any [[Actor]] in the persistent Level can be moved to the currently active level with [[Actor]] > right-click > Level > Move Selection To Current Level.
(
I don't see this option in Unreal Engine 5.3.
Is that feature newer or removed?
)

An [[Actor]] can be moved between Levels with Levels panel > a Level > right-click > Move Selected Actors To Level to move the [[Actor]]s selected in the [[Level Viewport]] and [[Outliner]] to the right-clicked Level.

You can show which Level each [[Actor]] is part of with [[Show]] > Advanced > Level Coloration.
Set the color for each Level in the Levels panel, in the far right.

# References

- [_Begin Play | World Building_ by Epic Games UE5.0](https://dev.epicgames.com/community/learning/tutorials/9k9B/unreal-engine-begin-play-world-building)
- [_An In-Depth Look at Environment Artist Based Tools_ > _Creating and Editing Levels_ by Epic Online Learning @ dev.epicgames.com 2021 UE4.27](https://dev.epicgames.com/community/learning/courses/3G/unreal-engine-an-in-depth-look-at-environment-artist-based-tools/L79/unreal-engine-creating-and-editing-levels)