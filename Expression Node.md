An expression node,  also called pure node, is a node that is not an [[Execution Node]].
Expression nodes provide one or more output pins.
An expression node evaluation is triggered by the [[Execution Node]] that the expression node tree that the expression node is part of is rooted at.
An expression node may be part of multiple expression node trees.
In that case the expression node is evaluated anew for every [[Execution Node]].
You can store the expression node's value in a variable if you only want to evaluate the expression node once,
or if you want to use a changing value multiple times.
The Random Float In Range is an example of an expression node that gives different values if evaluated multiple times.

Expression Nodes are used in both [[Blueprint Visual Script]] and the [[Material Graph]].
