A Timer is a way to schedule work to be done in the future.

Use a Set Timer By Event node to have a [[Blueprint Event]] be triggered at some point in the future.
The Set Timer By Event node has an Event input pin, marked with a red square.
[[Blueprint Event]] nodes have a corresponding red square next to their title.
Connecting the red square on the [[Blueprint Event]] node to the red square on the Set Timer By Event node causes that [[Blueprint Event]] to be called when the timer runs out.
One can drag from the Event input pin on the Set Timer By Event node and select Create Custom Event to create a new [[Blueprint Event]] node.

A Timer can be looping, meaning that it will be called repeatedly at even intervals until stopped.
Stop a Timer by calling the Clear Timer By Function Name node, pass in the name of the event called by the timer you wish to stop.
Don't know why the "By Function Name" version can stop "By Event" timers.
Don't know what happens if there are many timers calling the same [[Blueprint Event]].
