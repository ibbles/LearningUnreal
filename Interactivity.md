Interactivity means letting the player cause change in the game world.
It is the building blocks for gameplay.


# Triggering actions
## Overlap
One way to trigger an action is to listen for Begin and End Overlap events in a [[Blueprint Class]].
The volume to overlap with is often defined with a Collision Component, such as a Box or a Sphere.
An overlap event [[Blueprint Event]] node can be added to the [[Event Graph]] by right-click the Collision Component > Add Event > Add On Component Begin Overlap.

## Interaction Component
This is not a Component included in Unreal Engine, but a technique for [[Blueprint Communication]].
The idea is to add an Actor Component that acts as a [[Blueprint Event]] triggerer.
Actors that contains an instance of this Component can listen for the event, and outside sources can trigger the event.

Create a [[Blueprint class]] inheriting from [[Actor Component]]  named `BP_InteractionComponent`.
In the new Actor Component's Event Graph add a [[Custom Event]] named Start Interaction and one named Stop Interaction.
In the My Blueprint panel, under Event Dispatchers, add a new Event Dispatcher for each of the two Custom Events.
Or a single On Interact Event Dispatcher that has a Boolean input.
In the Interaction Component's [[Event Graph]], drag the Event Dispatcher from the My Blueprint panel to the graph and connect to the [[Custom Event]].

Add the Interaction Component to an [[Actor]] [[Blueprint Class]] that is the source of the interaction, such as a button or a lever.



# References
- [_Your First Game in UE5 | Tech Talk | State of Unreal 2022_ - Interaction Component, introduction by Unreal Engine @ youtube.com](https://youtu.be/Itd677YZi50?t=3680)
- [_Your First Game in UE5 | Tech Talk | State of Unreal 2022_ - Interaction Component, instructions by Unreal Engine @ youtube.com](https://youtu.be/Itd677YZi50?t=4120)

