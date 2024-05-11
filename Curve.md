A Curve is a visually defined `R → T` function where `T` can be e.g. float, Vector, or Linear Color.
It is controlled with interpolation points.
A Curve is a type of [[Asset]] created from the Miscellaneous section of the Create Asset context menu in the [[Content Browser]].

In the Curve editor press Enter to create new interpolation points.
Drag the points  to define (X, Y) pairs.
Use the two text inputs in the tool bar to explicitly set values for X and Y.

Right-click an interpolation point to change interpolation type:
- Linear: Linear interpolation between the previous node and this node.
- Constant: Maintain the previous point's Y value until the new point is reached.
- Break: 
- Auto: Provides a handle to control the tangent.

To access a float curve in C++ declare a [[Property]] of type `UCurveFloat*`.
To sample the function call `UFloatCurve::GetFloatValue(float X)`.
