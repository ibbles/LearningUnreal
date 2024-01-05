To create a Custom Event right-click the [[Event Graph]] and select Create Custom Event.
This create a red execution root node with an execute pin we can chain execution nodes from.

To trigger the event, i.e. run the execution nodes connected to the red execution root node, right-click the [[Event Graph]] and select Call Function > EVENT NAME.

If the Custom Event is in a parent class of the current [[Blueprint Class]], you can also select Add Event > Event EVENT NAME and then right-click the event and select Add Call To Parent Function.
