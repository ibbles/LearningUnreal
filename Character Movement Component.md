#Movement

A Component that the [[Character]] class has.
Provides basic walking/running/jumping functionality.

Has Details panel > Character Movement (Rotation Settings) > Orient Rotation To Movement.
Makes the [[Character]] turn towards the direction it is moving.
This may also cause a [[Camera]] attached to the [[Character]] to turn in the same direction.
If that is not desired then the Camera's, or a parent Scene Component in the hierarchy such as a Sprint Arm's, Rotation to be world/absolute instead of relative.
