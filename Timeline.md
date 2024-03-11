A Timeline is a [[Blueprint Visual Script]] node that has one or more value output pin that changes over time and an execution output pin that is called every tick.
The value that is output each tick is controlled by a [[Track]].
The [[Track]] maps **time values to output values**.
A Timeline has a **length**, which is the Timeline's duration in seconds.
A Timeline acts much like a **media player** in that it can be played, played from start, stopped, reversed, and jump to a specific time.
A Timeline can be used to **drive simple animation** where the animation state can be controlled with a parameter, such as a door opening.

**Create a new Timeline** by right-clicking in the [[Event Graph]] (not in a [[Blueprint Function]]) and select Add Timeline, give it a name.
Timelines cannot be added to functions, only the [[Event Graph]].
Edit the Timeline by double-clicking the Timeline node.
The Timeline also show up as a **variable** in the Components category.
On this variable we can call Play From Start.

# Timeline Editor
The Timeline Editor is a tab within the [[Blueprint Actor Editor]].
The length, or duration, of the Timeline is set at the top.
Tracks are added with the `+ Track` button.
Each Track has a name that is also the name of the Timeline node output pin in the [[Event Graph]].

The Track part of the Timeline editor contains a **curve editor**.
The curve editor contains key points that set the track value for specific time points in the timeline.
**Add a key** by right-click the curve editor and select Add Key To TRACK NAME.
You can also **Shift-click** to create a key.
We can position keys either by click-and-drag or by typing values into the Time and Value boxes at the top of the curve editor.
The value is interpolated between key points for time points between the key points.
We can control some aspects of the interpolation by right-clicking a key point and selecting an interpolation method.
The Break interpolation method adds tangent handles to the key point.

The view of the curve editor can be focused with the Expand Horizontally and Expand Vertically buttons at the top of the curve editor.



# Play Rate, Speed

We can set the playback speed of a Timeline in our Blueprint.
There is a member function for this named Set Play Rate.
We need a reference to the Timeline object to call the function.
Do not use the Timeline node itself for this.
Instead, the Timeline node has a name, Timeline by default.
This name is also the name of a variable, which can be found under Components in the [[Blueprint Variable]] section of the [[My Blueprint Panel]].
So right-click the [[Node Graph Editor]] and search for `Get Timeline`, your Timeline should show up, or drag it from the [[My Blueprint Panel]].
If you have renamed the Timeline node (From the right-click menu or with F2.) then search for that name instead.
Drag off of the output pin of the Timeline reference node and search for Set Play Rate.

If the Timeline has a length of 1.0 then we can use Set Play Rate as a duration control as follows:

![[Timeline__SetPlayRate.jpg]]

