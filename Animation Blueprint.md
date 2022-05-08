An Animation Blueprint contains logic deciding which animation a [[Skeleton Asset|Skeleton]] should be playing.

A [[Skeletal Mesh Component]] has Details panel > Animation > Anim Class that can be set to an Animation Blueprint.
That will make instances of the Actor containing the [[Skeletal Mesh Component]] play animations according to that Animation Blueprint.

Create a new Animation Blueprint with Content Browser > Animation > Animation Blueprint.
When creating a new Animation Blueprint you select a [[Skeleton Asset]] that the Animation Blueprint should control.
Animation Blueprints are typically named with a `ABP_` prefix.

The Animation Blueprint Editor contains
- a Viewport.
- a My Blueprint panel.
  Contains variables that can be set in the Event Graph.
- a Details panel.
- an Anim Preview E...
- an Animation Graph.
  Where we define which pose the skeleton should have in this frame.
- an Event Graph.
- an Asset Browser.
  Lists all the [[Animation Asset|Animation Assets]] available for the current [[Skeleton Asset]].

# Anim Graph
The Anim Graph is evaluated every tick to determine the current pose of the skeleton.
I think the tick rate can be reduced/controlled to improve performance.
The output of the Anim Graph is a pose.
A pose is a single frame of an animation, or a blend between frames of multiple animations.
An animation node is created by dragging an [[Animation Asset]] from the Asset Browser to the Anim Graph.
Connecting the animation node output to the output pose input will make the skeleton play that animation.


# State Machine
An Animation Blueprint can switch between different states, with different animations associated with each state.
Create a new State Machine by right-click in the Anim Graph and select Add New State Machine....
A State Machine appear as a node in the Anim Graph that has a pose output, which can be connected to the output pose node.

The State Machine contains a Visual Script Graph of its own, starting with an Execution Node called Entry.
A State Machine contains a collection of States.
Create a new State with right-click > Add State....

State have transition rules between each other.

# State
A State is its own Visual Script Graph.
It contains an Output Animation Pose node with a Result input pin.
We can drag an  [[Animation Asset]] from the Asset Browser into the State Visual Script Graph and connect to the Result pin.
We can drag an [[Animation Blend Space 1D]] from the Asset Browser into the State Visual Script Graph and connect to the Result pin.
A Blend Space 1D need to know the current axis value, which the Blend State Node takes as an input pin.

# Transition Rule
A Transition Rule determine if a State Machine can transition from one State to another.
A Transition Rule has a Visual Script Graph.
It contains a Result node with a Can Enter Transition boolean input pin.
Can read variables created in the My Blueprint panel and set in the Event Graph.

# Event Graph
The Event Graph contains a Blueprint Update Animation [[Blueprint Event]].
There is a Try Get Pawn Owner node that can be used to get the [[Pawn]], if any, that this Animation Blueprint is controlling.
It is common to get the velocity of the [[Pawn]] to decide which animation to play.
The Vector Length XY node will produce the ground speed of the Pawn.
