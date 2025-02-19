An AI Controller is used to have the computer control a [[Pawn]].
It is a subclass of [[Controller]], just like [[Player Controller]].
AI Controller instances only exist on the server.


# Creating An AI Controller

Create a new AI Controller by Content Browser > right-click > Create Basic Asset > Blueprint Class > search AI Controller.
AI Controller assets names are often prefixed with `BP_`.

An AI Controller is a type of Actor, so the editor for the AI Controller is the regular [[Blueprint Actor Editor]], with a [[Components Panel]], a [[My Blueprint Panel]], and so on.


# Defining The Behavior

We can get a reference to the [[Pawn]] being controlled, if any, with the Get Controlled Pawn function.


## Blueprint Control

One way an AI Controller controls a [[Pawn]] is with the AI Move To node.
The AI Move To node takes a [[Pawn]] to move and a destination in world coordinates to move to.


## Behavior Tree

Run a [[Behavior Tree]] with the Run Behavior Tree function.
A [[Behavior Tree]] is an [[Asset]] that describe a tree of tasks that the [[Pawn]] should perform.


# Assigning An AI Controller To A Pawn

A particular [[Pawn]] instance is assigned an AI Controller type from [[Details Panel]] > Pawn > AI Controller Class.
Also set Auto Possess AI at the same place.


# Navigation

An AI can only move through areas that have navigation data in the form of a [[Navigation Mesh]].
Unreal Engine will automatically create a [[Navigation Mesh]] within a Nav Mesh Bounds Volume, which can be created from the Place mode.
The generated mesh can be visualized by selecting Viewport > Show > Navigation, or by pressing P.


# References

- 1: [_Your First Game in UE5 | Tech Talk | State of Unreal 2022_ by Unreal Engine @ youtube.com](https://youtu.be/Itd677YZi50?t=3149)
