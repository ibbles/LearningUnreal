Also called Packed Level Instance.

Kind of like a [[Level Instance]], but can only contain [[Static Mesh Actor]]s.
The [[Static Mesh Actor]]s are replaced by [[Instanced Static Mesh]] Components in a Blueprint that Packed Level Actor creator creates.
The purpose is to improve rendering performance.
I don't know if instancing is shared among multiple instances of the Packed Level Actor.
I hope so.


# Use Cases

Intended to be used for dense assemblies, i.e. groups of many [[Static Mesh]]es.
Can be used to create a building that is repeated multiple times across a [[Level]].
Made for things that don't change during runtime.
If something needs to change in response to game logic then use a [[Level Instance]] instead of a Packed Level Instance.


# Contents

A Packed Level Actor has a reference to a [[Blueprint Class]].
This [[Blueprint Class]] is generated for you and contains [[Instanced Static Mesh Component]]s.
It inherits from the Packed Level Actor base class.
A Packed Level Actor has a reference, called World Asset, to a [[Level]] [[Asset]].
Not sure what the [[Level]] [[Asset]] is used for.


# Create A Packed Level Actor

Can be created from an [[Actor]] selection in the [[Level Viewport]].
- Create a bunch of [[Static Mesh Actor]]s.
- Select them.
- Right-click > _Actor Options_ > Level > Create Packed Level Actor.

A settings window appears, click OK.
A Save Level As dialog window opens, type a name and click Save.
A Save Asset As dialog window opens, type a name and click Save.
The [[Asset]] is a [[Blueprint Class]] inheriting from Packed Level Actor.
The selected [[Static Mesh Actor]]s are replaced by the [[Blueprint]].
The [[Blueprint Class]] contains an [[Instanced Static Mesh]].


# Create A Packed Level Actor Instance

The [[Blueprint Class]] can be dragged into the [[Level Viewport]] to create additional instances of the Packed Level Actor.
These can be transforms with the [[Transform Gizmo]] as usual, and also duplicated.


# Edit a Packed Level Actor Instance

The Packed Level Actor subclass can be edited by [[Level Viewport]] > Packed Level Actor > right-click > Level > Edit.
This desaturates everything in the [[Level Viewport]] except for the [[Static Mesh]]es that make up the Packed Level Actor.
The individual [[Static Mesh]]es can be moved, rotated and duplicated from the [[Level Viewport]].
When done do [[Level Viewport]] > Packed Level Actor > right-click > Level > Commit.
This will update all instances of that Packed Level Actor in the [[Level]].

If the World Asset is modified then we need to right-click any instance in the [[Outliner]] and select Update Packed Blueprint.
That will update all instances of the Level Instance in the [[Level]].


# Break A Packed Level Actor Instance

(
Not sure if this is only for [[Level Instance]], or also for Packed Level Actor.
)

If you want to modify a particular instance of a Packed Level Instance then you can right-click > Level (Instance?) > Break > Break Level Instance.
This will take all the [[Actor]]s in the Level Instance and create them as regular free-standing [[Actor]]s.
There is a Levels setting since Packed Level Instances can be nested.


# Nesting Inside A Level Instance

A [[Level Instance]] can contain a Packed Level Actor.
The example given was a palm tree where a Packed Level Actor was used to create a collection of fern leaves from a small set of [[Static Mesh Asset]]s combined in various ways.


# References

- [_Large worlds in UE5: A whole new (open) world | Unreal Engine_ by Epic Online Learning 2021 UE5.0](https://dev.epicgames.com/community/learning/talks-and-demos/KBe/large-worlds-in-ue5-a-whole-new-open-world-unreal-engine)
- [_Building Open Worlds in Unreal Engine 5 | Unreal Fest 2022_ by Epic Online Learning @ dev.epicgames.com/talks-and-demos](https://dev.epicgames.com/community/learning/talks-and-demos/LLM5/building-open-worlds-in-unreal-engine-5-unreal-fest-2022)
- [_Building Bigger: Changing Your Workflow for Building Worlds instead of Scenes | Unreal Fest 2023_ by Chris Murphy @ dev.epicgames.com/talks-and-demos 2024 UE5.3](https://dev.epicgames.com/community/learning/talks-and-demos/jwlJ/unreal-engine-building-bigger-changing-your-workflow-for-building-worlds-instead-of-scenes-unreal-fest-2023)


