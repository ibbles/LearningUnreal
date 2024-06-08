Another variant it [[Packed Level Actor]].
Use Level Instance when the content needs to change in response to game logic.
Use [[Packed Level Actor]] when the content is fixed, unchanging.

A reusable collection of [[Actor]]s stored as an [[Asset]].
A way to spawn a [[Level]] inside another [[Level]].
A type of [[Actor]] that brings in an entire [[Level]] into another.
For example, can have a [[Level]] that is the main parts of a building and bring that in multiple times.
And then have other [[Level]]s for different types of room contents.
Level Instances can be nested inside each other.

A Level Instance can be duplicated (Alt + drag).

# Create A Level Instance Asset

Select a bunch of [[Actor]]s in your level > right-click > _Actor Options_ > Level > Create Level Instance.
A settings window opens:
- External Actors:
- Pivot Type:
- Pivot Actor:

A new [[Level]] asset is created containing those [[Actor]]s.
I assume the [[Actor]]s will be centered, i.e. placed near the origin of the new [[Level]] and not keep their World Location from the original [[Level]].


# Create A Level Instance Instance

Create from the Place Actors panel, named Level Instance.
Has [[Details Panel]] > Level Instance > World Asset which is a reference to a [[Level]] [[Asset]].
All the [[Actor]]s that the [[Level]] [[Asset]] contains will be placed around the Level Instance [[Actor]].

Will create actual, full [[Actor]]s that are individually listed in the [[Outliner]] grouped under the Level Instance [[Actor]].
These [[Actor]]s are not editable (I think).
To edit them you must edit the Level Instance [[Asset]], which will propagate to all instances.
See _Modify A Level Instance Asset_.

# Modify A Level Instance Asset

One way to modify a Level Instance [[Asset]] is to open the [[Level]] and edit it just like any other [[Level]].

A faster way is with the in-place Edit mode.
With a Level Instance instance selected hit CTRL + E to enter Edit mode.
This will temporarily made the editor "descend" into the Level Instance.
[[Actor]]s not part of the Level Instance will be grayed out in the [[Outliner]].
Make changes to the [[Actor]]s in the level instance.
You can duplicate them, move then around, change settings in the [[Details Panel]], etc.
Commit your changes and leave Edit mode with the Escape key.


# Break A Level Instance Instance

A Level Instance Instance can be broken apart.
This means that all the [[Actor]]s in the Level Instance are created as regular [[Actor]]s in the level and the Level Instance Instance removed.
You can then edit the new [[Actor]]s just like any other regular [[Actor]] without affecting the Level Instance.

Use this to have starting point templates for things.
Create a Level Instance that is close to what  you often need.
For every new variant, create a Level Instance Instance, break the instance, and make any changes you need in this particular case.


# Embedded Mode VS Level Streaming Mode

The Level Instance can be realized in different ways during runtime.
Which variant is used depend on if One File Per Actor is enabled or not:
- Enabled: Embedded Mode.
- Disabled: Level Streaming mode.

In embedded mode the Level Instance is just a modeling tool.
At runtime the constituent [[Actor]]s are created in the enclosing level as-if they had been created there directly by hand.
The Level Instance [[Actor]] doesn't exist at all.
Per-level settings stored in `AWorldSettings` set in the [[Level]] [[Asset]] will not be included.
[[Level Blueprint]] will not be included.

In level streaming mode the Level Instance is streamed like a regular level.
The Level Instance [[Actor]] is loaded just like any other [[Actor]] when  its [[World Partition]] cell is loaded and brings with  it all the sub-[[Actor]]s that are in  the [[Level]] [[Asset]].
The `AWorldSettings` [[Actor]] will be included.
[[Level Blueprint]] will be included.

I don't know how this plays out when [[World Partition]] is disabled.


# Current Level Instance

A Level Instance can be made current.
When there is a current Level Instance then any new [[Actor]] that is created is placed in that Level Instance.
(TODO Figure out how to make a Level Instance current.)


# Level Instance Zoo / Gallery

If you have many Level instances then I suggest creating a Level Instance  Zoo / Gallery.
This isn't really an engine thing.
It is just a [[Level]] for the developers' convenience where all the Level Instances are laid out for an easy overview.
Just like is common for asset packs on the [[Unreal Marketplace]].


# References

- [_Large worlds in UE5: A whole new (open) world | Unreal Engine_ by Epic Online Learning 2021 UE5.0](https://dev.epicgames.com/community/learning/talks-and-demos/KBe/large-worlds-in-ue5-a-whole-new-open-world-unreal-engine)
- [_Building Open Worlds in Unreal Engine 5 | Unreal Fest 2022_ by Epic Online Learning @ dev.epicgames.com/talks-and-demos](https://dev.epicgames.com/community/learning/talks-and-demos/LLM5/building-open-worlds-in-unreal-engine-5-unreal-fest-2022)
- [_Building Bigger: Changing Your Workflow for Building Worlds instead of Scenes | Unreal Fest 2023_ by Chris Murphy @ dev.epicgames.com/talks-and-demos 2024 UE5.3](https://dev.epicgames.com/community/learning/talks-and-demos/jwlJ/unreal-engine-building-bigger-changing-your-workflow-for-building-worlds-instead-of-scenes-unreal-fest-2023)



