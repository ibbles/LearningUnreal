An AI Controller is used to have the computer control a [[Pawn]].
It is a subclass of [[Controller]], just like [[Player Controller]].

Create a new AI Controller by Content Browser > right-click > Create Basic Asset > Blueprint Class > search AI Controller.
AI Controller assets names are often prefixed with `BP_`.

A particular [[Pawn]] instance is assigned an AI Controller type from Details panel > Pawn > AI Controller Class.

An AI Controller is a type of Actor, so the editor for the AI Controller is the regular [[Blueprint Actor Editor]], with a Components panel, a My Blueprint panel, and so on.

We can get a reference to the [[Pawn]] being controlled, if any, with the Get Controlled Pawn node.

One way an AI Controller controls a [[Pawn]] is with the AI Move To node.
The AI Move To node takes a [[Pawn]] to move and a destination in world coordinates to move to.

An AI can only move through areas that have navigation data in the form of a nav mesh.
Unreal Engine will automatically create a nav mesh within a Nav Mesh Bounds Volume, which can be created from the Place mode.
The generated mesh can be visualized by selecting Viewport > Show > Navigation, or by pressing P.

AI Controller instance only exists on the server.

# References

[_Your First Game in UE5 | Tech Talk | State of Unreal 2022_ by Unreal Engine @ youtube.com](https://youtu.be/Itd677YZi50?t=3149)
