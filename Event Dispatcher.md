An Event Dispatcher is something that other objects can listen to.
Other objects can get a callback when the Event Dispatcher is triggered.
We say that the Event Dispatcher dispatches an [[Event]].

An Event Dispatcher is a mechanism used to implement an event-driven system.
Another common name for this mechanism is Publisher / Subscriber or Observer Pattern.
The purpose is to avoid repeated costly state checking and instead only updating something, such as a [[Widget]], when the underlying state changes.

The Event Dispatcher may pass data, called parameters, with the [[Event]].

An Event Dispatcher is a type of [[Delegate]].

# Blueprint

## Creating An Event Dispatcher

An Event Dispatcher is created from the [[My Blueprint Panel]] of the [[Blueprint Class Editor]].
Give the new Event Dispatcher a name.
It is common to give Event Dispatchers a name that starts with "On".
To add an input, a.k.a. parameter, to the Event Dispatcher click [[Details Panel]] > Inputs > + button.

Trigger an Event Dispatcher by dragging it from the My Blueprint > Event Dispatchers list into the [[Event Graph]] and select Call from the list that pops up.
Any listeners registered with the Event Dispatcher will be executed.
If the Event Dispatcher has any parameter then those appear as input pins on the Call node.
(
I assume.
)

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


## Listening To An Event Dispatcher

To listen to an Event Dispatcher we need to get a hold of if,
often by getting a reference to the object that owns it.
If there are many instances of the Event Dispatcher owning type then this process must be repeated for every instance we are interested in.

Once you have a reference to the object owning the Event Dispatcher in the [[Event Graph]]
drag off of the reference output pin and
search for Bind Event To EVENT NAME
where EVENT NAME is the name you gave the Event Dispatcher when it was created.
This create a Bind Event node that has an [[Event]] input pin that can be connected to the event you want to run when the Event Dispatcher is triggered.
A short-hand is to select Assign EVENT NAME instead of Bind Event To EVENT NAME,
that creates connects a [[Custom Event]] for you.
See _Blueprint Communication_ below for a step-by-step example.

## Blueprint Communication

An Event Dispatcher can be used for [[Blueprint Communication]].
Create one [[Blueprint Class]], let's call it `BP_Dispatcher` for example, that contains the Event Dispatcher, created as described above.
For this example let's call it `MyEvent`.
Create another [[Blueprint Class]], let's call it `BP_Listener` for example, that has a [[Blueprint Variable]] whose type is a reference to the first [[Blueprint Class]].
Make the variable public so that a level designer can chose which instances of the two [[Blueprint Class]]es should communicate.
In listener [[Blueprint Class]] drag the `BP_Dispatcher` reference variable into the [[Event Graph]] to get the reference and from it drag and select Bind Event To My Event.
A node Bind Event node appears, which has an Event input pin.
Drag of off the Event input pin an select Create Custom Event.

Now, whenever the Dispatcher [[Blueprint Class]] triggers its Event Dispatcher the [[Custom Event]] in the listener [[Blueprint Class]] will be run. 


# C++

Not sure how to bind a C++ function to a Blueprint Event Dispatcher, or how to declare a C++ class with an Event Dispatcher.
There is a similar concept called [[Delegate]] that is designed for use in C++.

# Input

Event Dispatchers can send arguments along with the event when triggered.
Select the Event Dispatcher in the My Blueprint panel and at the top of the Details panel there is a list of inputs.
This will cause every Call node for that Event Dispatcher to include input pins for those inputs.


# References

- [_Blueprint Communication > Event Dispatchers_ by Epic Games @ dev.epicgames.com 2022](https://dev.epicgames.com/community/learning/courses/LWv/unreal-engine-blueprint-communication/b7yv/unreal-engine-event-dispatchers)
- [_Breaking Down the Components of Gameplay_ > _Component and Interface Communication_ by Epic Games, Rob @ dev.epicgames.com 2021](https://dev.epicgames.com/community/learning/courses/mo/unreal-engine-breaking-down-the-components-of-gameplay/bq0/unreal-engine-component-and-interface-communication)
- [_Event Dispatchers in UE5 are EASY! Simple STEP-BY-STEP Tutorial in Blueprint!_ by Unreal Dev Hub @ youtube.com 2024](https://www.youtube.com/watch?v=uBl9kIdOT-k)
- [_The Most Common Mistake Beginners Make in Unreal Engine | UE5_ by Ali Elzoheiry @ youtube.com 2024](https://www.youtube.com/watch?v=YFtLd-bKl-U)
