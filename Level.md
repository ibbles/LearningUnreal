A Level is a scene of sorts, a stage where we can put [[Actor|Actors]].
It is where everything made for the game comes toghether.
Closely related to [[Map]].
The difference between a Level and a [[Map]] is unclear to me.

Level is an [[Asset]] which means that they live in the [[Content Browser]].
The level and all its contents, i.e. the Actors, can stored in a single `.umap` file.
The level and it's contents can also be stored separate from either other by enabling [[One File Per Actor]].
A level may also be split up into multiple chunks using [[World Partition]].
Whether or not a level uses [[World Partition]] is selected when the level is created, or with Top Menu Bar > Tools > Convert Level.
[[World Partition]] controls streaming.

Create a new Level by right-click in the [[Content Browser]] and select Level.

An [[Actor]] placed in the level, and any [[Actor Component]] it contains, is called an instance and the properties of that particular instance can be modified from the [[Level Editor]] [[Details Panel]] when the [[Actor]] or [[Component]] is selected.

The main Unreal Editor windows is actually the [[Level Editor]] and the window showing the scene is the [[Level Viewport]].
When you open a Level by double-clicking it in the [[Content Browser]] then the Level currently open is closed and the double-clicked one is opened.

An Actor can be added to the level using [[Main Tool Bar]] > Place Actor > drag an Actor type into the [[Level Viewport]] where you want it.
The Place Actor menu is searchable, meaning you can type the name of the Actor type you want to filter the list.
Each Actor type may be part of multiple categories and may therefore appear multiple times in the list.

An Actor can be added to the level by dragging an [[Asset]] from the [[Content Browser]] onto the [[Level Viewport]].

A level may require building before it can be used, depending on what features the level uses.
The following features require building:
- [[Baked Lighting]]
- Some properties of [[Landscape]].
- [[HLOD]].

You can set the Level that is opened when you open your project in Unreal Editor and the Level that the game starts at in the [[Project Settings]], under Project > Maps & Modes > {Editor Startup Map, Game Default Map}.


# References

- [_Begin Play | World Building_ by Epic Games UE5.0](https://dev.epicgames.com/community/learning/tutorials/9k9B/unreal-engine-begin-play-world-building)
