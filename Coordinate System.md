Unreal Engine uses a very peculiar coordinate system.
It is mostly a left-handed coordinate system, meaning the the directions of the principal axes X, Y, and Z follow the direction of the left hand's thumb, index, and middle finger, respectively.

- X: Forward
- Y: Right
- Z: Up

Rotations, however, are a mixture between left-handed rotations and right-handed rotations

- X: Right-handed
- Y: Right-handed
- Z: Left-handed

Transformations and objects with transformations has getter functions such as `GetForwardVector` and `GetUpVector`,
that returns the local forward or up vector in the world coordinate frame.

The length [[Units|unit]] of Unreal is centimeter.
# References

- [_Becoming an Environment Artist in Unreal Engine_ > _Scale Prep for Unreal Usage_ by Epic Games @ dev.epicgames.com/courses 202 UE4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/JGl/unreal-engine-scale-prep-for-unreal-usage)

