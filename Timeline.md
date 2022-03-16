A Timeline is a [[Blueprint Visual Script]] node that has an value output pin that changes over time and an execution output pin that is called every tick.
The value that is output each tick is controlled by a [[Track]].
The [[Track]] maps **time values to output values**.
A Timeline has a **length**, which is the Timeline's duration.
A Timeline acts much like a **media player** in that it can be played, played from start, stopped, reversed, and jump to a specific time.
A Timeline can be used to **drive simple animation** where the animation state can be controlled with a parameter, such as a door opening.

**Create a new Timeline** by right-clicking in the [[Event Graph]] and select Add Timeline.
Timelines cannot be added to functions, only the [[Event Graph]].

