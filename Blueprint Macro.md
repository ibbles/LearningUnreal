A Blueprint Macro is similar to a function in that it gives a collection of Blueprint nodes a name and that it can be called from other places.

There are some things that can and cannot be done is a Macro compared to a function.

A Macro can have temporary variables.
Allocate a temporary variable with the Local TYPE family of nodes.
The variable can be set by connecting it to an Assign node.
The  variable is read by connecting it to an input pin of the correct type.
