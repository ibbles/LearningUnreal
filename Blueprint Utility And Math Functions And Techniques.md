# Map Range Clamped
A way to map one range onto another, i.e. scale an input value that is within so range to instead be within some other range.

# Flip Flop
A Flip Flop is a node that has on input execution pins and two output execution pins.
The execution flow will take either output pin and flip between them every time the node is executed.
This can be used for toggle operations, such as to open/close a door.

# Vector Snapped To Grid
Round a Vector to an arbitrary grid size.
This is useful when placing Actors or Components in a regular grid.
Unfortunately the grid cells must have the same size in all dimensions, i.e, they must be cubes.

# Get Unit Direction
Given two Locations, From and To, return a unit vector pointing from From to To.
Useful to pass to Add Movement Input.
