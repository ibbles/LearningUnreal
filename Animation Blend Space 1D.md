An Animation Blend Space 1D blends poses between two or more animations.
It has an input value that is used when deciding how much of each animation to blend into the result at any point in time.
The input value is controlled from an [[Animation Blueprint]].
The animations to blend between is put on an axis along with the input value.
The closer the input value is  to an animation on the axis, the more of that animation is included in the final pose.

An Animation Blend Space 1D asset can be creation with Content Browser > right-click > Animation > Blend Space 1D.
A Blend Space 1D is associated with a [[Skeleton Asset]], so one must be selected when creating the Blend Space.
Blend Space names are typically prefixed with `BS_`.

The Blend Space 1D editor contains
- a Viewport.
- an Asset Details panel.
- a Details panel.
- an Asset Browser.
  List of Animation Sequences available for the [[Skeleton Asset]].
- a Blend Space (name?) panel.

The axis of the Blend Space 1D can be named with Asset Details > Axis Settings > Horizontal Axis > Name.
The range of the axis can be set with Asset Details > Axis Settings > Horizontal Axis > {Minimum, Maximum} Axis Value.
Animation Sequences can be dragged from the Asset Browser into the Blend Space panel.
They will show up as points in the Blend Space.
Placing the Animation Sequence points on the axis control how much of that animation is included for a particular input value.

A Blend Space 1D can be dragged into the Visual Script Graph of an [[Animation Blueprint]] State.
It shows up as a node with an input pin for the current axis value.

# Preventing feet sliding
A common use case for animation blending is between locomotion variants for different speeds, such as walking and running.
Care must be taken to match the speed of the Actor through the world with the animation blending so that the feet of the [[Skeletal Mesh Asset]] doesn't slide.
Unreal Engine has built-in tools for this.
To let Unreal Engine control the animation blending set the following settings in Asset Details > Analysis:
- Set Horizontal Axis Function to Locomotion.
- Set Bone/Socket 1 to the left foot bone in the [[Skeletal Mesh Asset]].
- Set Bone/Socket 2 to the right foot bone in the [[Skeletal Mesh Asset]].
- Click the circular arrow button next to Analyze `NUMBER` Samples.

