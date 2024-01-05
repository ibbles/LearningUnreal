An Event Dispatcher is something that other objects can listen to.
Other objects can get a callback when the Event Dispatcher is triggered.

An Event Dispatcher is created from the My Blueprint panel of the [[Blueprint Editor]].

Trigger an Event Dispatcher by dragging it from the My Blueprint > Event Dispatchers list into the [[Event Graph]] and select Call from the list.
Any listeners registered with the Event Dispatcher will be executed.

When dragging the Event Dispatcher from the My Blueprint panel to the [[Event Graph]] and selecting Event from the list we will get a red [[Blueprint Event]] node.
This event node can be bound to the Event Dispatcher to connect them.
Drag the Event Dispatcher from the My Blueprint panel and select Bind from the list.
We get an executable node that has an event input pin.
Connect the [[Blueprint Event]] node to the event input pin of the Bind node.
Connect the input execution pin on the Bind node to something that is executed at startup, such as a Begin Play event.
That registers the [[Blueprint Event]] with the Event Dispatcher so that the [[Blueprint Event]] is executed when the Event Dispatcher is triggered.
Now the node sequence rooted at the event will be executed whenever the Event Dispatcher is triggered.

Instead of selecting Event and Bind as two actions we can select Assign from the list when dragging the Event Dispatcher from the My Blueprint panel to the [[Event Graph]].
This will create both nodes and connect them.


# Input

Event Dispatchers can send arguments along with the event when triggered.
Select the Event Dispatcher in the My Blueprint panel and at the top of the Details panel there is a list of inputs.
This will cause every Call node for that Event Dispatcher to include input pins for those inputs.

# References

- [_Blueprint Communication > Event Dispatchers_ by Epic Games @ dev.epicgames.com 2022](https://dev.epicgames.com/community/learning/courses/LWv/unreal-engine-blueprint-communication/b7yv/unreal-engine-event-dispatchers)

