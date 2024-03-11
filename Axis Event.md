An axis Event is a continuous events that trigger every frame and that comes with an axis value in the range [-1.0, 1.0].
Another type of event is the [[Action Event]].
Both are types of [[Input Event]].

With each event triggering is associated an axis value.
The axis value represent how far a gamepad stick, or similar, has been moved from its neutral position along an axis.
Axis Events nodes have one execution output pin and one float output pin providing the axis value.

Axis event [[Input Bindings]] are set up in [[Project Settings]] > Engine > Input  > [[Axis Mappings]].
When an Axis Mapping is set up for a hardware axis, such as a gamepad stick, then the axis value is taken from the hardware.
If a key or button is used, then we must set the axis value that should be used from  within the [[Axis Mappings]] in the [[Project Settings]].
This is called the _scale_ of the key for the Action Mapping.
The scale can be negative, meaning that we can have a single Axis Mapping for both forward and backwards,
and we should provide one key for forward and another key, with a negate scale, for backward.

[[Input Event]] can be listened to both in [[Blueprint Visual Script]] and with [[Action Event In C++]].

