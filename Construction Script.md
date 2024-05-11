A [[Blueprint Class]] may have a Construction Script.
This is similar to a C++ constructor in that it is run when an instance of the class is created.
The Construction Script is a tab next to the [[Event Graph]]

A Construction Script is implemented as a [[Blueprint Visual Script]], very much like any other [[Blueprint Function]].

Remember to call the **Parent Construction Script**.
If there is no  node for the Parent Construction Script then create one by clicking the Construction Script start node and select Add Call To Parent Function.
It is often a good idea to call the parent's Construction Script even if it doesn't do anything because it might do something in the future.

There is a tab in the [[Blueprint Actor Editor]] for the Construction Script.
The tab can be re-opened by double-clicking My Blueprint panel > Functions > ConstructionScript.

By default the Construction scrip is run continuously when an Actor Instance is **dragged**.
This can be disabled by unchecking Toolbar > Class Setting > Blueprint Options > Run Construction Script on Drag.
The construction script is not run when a [[Scene Component]] within an Actor Instance is dragged.

A [[Property]] edited from the Construction Script becomes marked as [[User Construction Script Modified Property]], or UCSModified for short.
