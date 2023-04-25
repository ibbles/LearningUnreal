Unreal Engine uses a very peculiar coordinate system.
It is mostly a left-handed coordinate system, meaning the the directions of the principal axes X, Y, and Z follow the direction of the thumb, index, and middle finger, respectively.

- X: Forward
- Y: Right
- Z: Up

Transformations and objects with transformations has getter functions such as `GetForwardVector` and `GetUpVector`,
that returns the local forward or up vector in the world coordinate frame.

