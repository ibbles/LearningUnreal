A Level Blueprint is a Blueprint associated with a particular [[Level]].
It is not a [[Blueprint Class]], just an [[Event Graph]], [[Blueprint Function|Blueprint Functions]], and [[Blueprint Variable|Blueprint Variables]].
It cannot have [[Component|Components]] since it is not an [[Actor]].

The Level Blueprint can reference Actors inside the [[Level]].
Have the [[Actor]] selected in the [[Level Viewport]] and right-click inside the Event Graph of the Level Blueprint.
You will see Create A Reference To ACTOR_NAME.
From the same menu we can also add event listeners for the [[Event|Events]] that the [[Actor]] exposes.
For example Begin Overlap.

The events within a Level Blueprint executes within the context of an instance of that level.
This means that it has access to and can manipulate object instances in the level.
To create a reference to any object in the level select the object in the [[Level Viewport]], right-click the Level Blueprint [[Node Graph Editor]], and select Create A Reference To NAME.
You can also create [[Blueprint Event|Blueprint Events]] or call functions from the same menu.


# References

- [_Blueprint Communication - Level Blueprint_ by Epic Online Learning, Mathew Wadstein. 2022. @ dev.epicgames.com](https://dev.epicgames.com/community/learning/courses/LWv/unreal-engine-blueprint-communication/8nv8/unreal-engine-level-blueprint)

