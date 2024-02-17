[[Interactivity]]
[[Blueprint Interface]]

# Level Blueprint

Objects within a [[Level]] can communicate via the [[Level Blueprint]].
In that case it's not really the objects that are communicating, it is the [[Level Blueprint]] that orchestrates everything.
Simple to set up, but code is difficult to re-use.
Should normally not be used much.
Only suitable when there is some functionality that  only makes sense within a single level.


# Within A [[Blueprint Class]]

If the objects communicating are part of the same [[Blueprint Class]] then the communication can be handled by that class.
If we have a class hierarchy then the parent class can provide functionality in the form of a [[Custom Event]] and the child class trigger that custom event at the appropriate times.


# Public Variables

See [[Blueprint Variable]].
To communicate between Blueprint instances one Blueprint, the source, needs to know about the other Blueprint instance, the target.
For this we can use a [[Blueprint Variable]] of type Object Reference.
Make it Instance Editable to make it possible for a level designed to associate a particular source instance with a particular target instance.
To talk to the target object from inside the source object drag the object reference variable into the [[Event Graph]].
Drag from the output pin of the object reference and type in the name of the [[Custom Event]] or [[Blueprint Function]] to call.


# Casting

See [[Cast]].
If you have a generic reference type then you can use casting to get a more specific reference type.
For example the [[Begin Overlap]] event provides the Other Actor and Other Component pins.
These have the types [[Actor]] and [[Scene Component]].
These can be cast to a more specific type in order to use variables and call functions provided by that more specific type.
A cast can fail, meaning that the actual object wasn't of the type we tried to cast it to.
The Cast node has two output execution pins, one is triggered if the cast was successful and the other is triggered if the cast failed.


# Blueprint Interface

See [[Blueprint Interface]].


# Event Dispatcher

See [[Event Dispatcher]], and also [[Delegate]].
This is useful to update the in-game UI.
Have the UMG widget's On Construct event bind to an Event Dispatcher in the [[Game Mode]].
The event in the UMG can update a variable, or set some text, whenever the event is triggered.
Event parameters can be used to communicate the new value from the [[Game Mode]] to the UI.


# References

- [_Blueprint Communication_ by Epic Online Learning, Mathew Wadstein. 2022. @ dev.epicgames.com](https://dev.epicgames.com/community/learning/courses/LWv/unreal-engine-blueprint-communication/ypKl/unreal-engine-blueprint-communication-overview)

