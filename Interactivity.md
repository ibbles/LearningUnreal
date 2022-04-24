Interactivity means letting the player cause change in the game world, to give agency.
It is the building blocks for gameplay.


# Triggering actions
These are a few ways we can implement interactivity.

## Overlap
One way to trigger an action is to listen for Begin and End Overlap events in a [[Blueprint Class]].
The volume to overlap with is often defined with a Collision Component, such as a Box or a Sphere.
An overlap event [[Blueprint Event]] node can be added to the [[Event Graph]] by right-click the Collision Component > Add Event > Add On Component {Begin, End} Overlap.

## Interaction Component

Based on [Your First Game in UE5 | Tech Talk | State of Unreal 2022, 1:08:40 @ youtube.com](https://youtu.be/Itd677YZi50?t=4120)

A technique for building an interaction system within an application.
This is not a Component included in Unreal Engine, but a technique for [[Blueprint Communication]].
The idea is to add an Actor Component that acts as a [[Blueprint Event]] triggerer.
Actors that contains an instance of this Component can listen for the event, and outside sources can trigger the event.

Create a [[Blueprint class]] inheriting from [[Actor Component]]  named `BP_InteractionComponent`.
In the new Actor Component's Event Graph add a [[Custom Event]] named Start Interaction and one named Stop Interaction.
In the My Blueprint panel, under Event Dispatchers, add a new Event Dispatcher for each of the two Custom Events.
Or a single On Interact Event Dispatcher that has a Boolean input.
Name the [[Event Dispatcher]] On Interact or something like that.
In the Interaction Component's [[Event Graph]], drag the Event Dispatcher from the My Blueprint panel to the graph to call it and connect to the [[Custom Event]].

Add the Interaction Component to an [[Actor]] [[Blueprint Class]] that is the source of the interaction, such as a button or a lever.
Drag the Interaction Component into the [[Event Graph]] and drag from its output pin and find the name of the first [[Custom Event]] to call it.
Whenever the interaction should take place have the execution reach the call node just created.
Repeat for the second [[Custom Event]] when the interaction should stop.

In the [[Actor]] that should listen for and react to the player interactive with the button or lever.
Create a new variable of type [[Actor]] Reference. The level designer will set this variable to the event source [[Actor]], i.e. the button or lever.
The [[Actor]] Reference variable must be marked Instance Editable since each instance of the owning [[Actor]] can be triggered by a different interactable [[Actor]].
On the [[Actor]] Reference call Get Component By Class and select `BP_InteractionComponent` for Component Class.
From the `BP_InteractionComponent` instance we can call Assign EVENT DISPATCHER, where EVENT DISPATCHER is On Interact in our example.
This will create a new [[Blueprint Event]] in this [[Actor]] that is executed when the `BP_InteractionComponent`'s Event Dispatcher is triggered.
Any parameters on the Event Dispatcher will show up as output pins on the [[Custom Event]].
Perform any gameplay logic you intent to have happen as a response to pushing the button or pulling the lever.
In the Level Editor, select an instance of the listening [[Actor]] and in the Details panel find the [[Actor]] Reference variable and set  it to some other [[Actor]] in the level that has a `BP_InteractionComponent`.

# References
- [_Your First Game in UE5 | Tech Talk | State of Unreal 2022_ - Interaction Component, introduction by Unreal Engine @ youtube.com](https://youtu.be/Itd677YZi50?t=3680)
- [_Your First Game in UE5 | Tech Talk | State of Unreal 2022_ - Interaction Component, instructions by Unreal Engine @ youtube.com](https://youtu.be/Itd677YZi50?t=4120)

