A [[Blueprint Class]] may contain functions implemented in [[Blueprint Visual Script]].

In the [[Blueprint Actor Editor]], **add a new function** to the current Blueprint Class with My Blueprint panel > Functions > `+ Function`.

A Blueprint Function is implemented as a [[Blueprint Visual Script]].
These consists of a network of nodes that are executed from the function's start node to one of possible several Return Node.

A Blueprint Function can take parameter **inputs** and return **outputs**.
The input and output values can be of the [[Blueprint Datatypes]].
With the function editor open add or remove inputs and outputs from the corresponding categories in the Details panel.
The inputs appear as output pins on the function's start node.
The outputs appear as input pins on the function's Return Node.

If your [[Blueprint Class]] overrides a function defined in the parent class then you can add a call to the parent class' implementation in your function implementation by right-clicking the functions start node and selecting Add Call To Parent Function.

A Blueprint Function can contain **local [[Blueprint Variable]]**.
Local variables are only accessible while executing the function.
Local variables are listed in My Blueprint > Local Variables.
This category is only shown if a Blueprint Function is open.
It is now shown when e.g. the Viewport or the Event Graph is open.
Create a **new local variable** by clicking the `+` next to My Blueprint > Local Variables.
Create a **new local variable** by right-clicking a node output pin and select Promote To Local Variable.
