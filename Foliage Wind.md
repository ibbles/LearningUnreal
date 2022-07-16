Foliage wind is implemented in the [[Material]] used on the [[Foliage]].
We make the foliage sway by plugging something into the [[World Position Offset]] input pin on the [[Material Output Node]].
There are many ways to compute the World Position Offset value.

One is to use the built-in Simple Grass Wind node.
This node makes the entire foliage sway, it doesn't know which parts of the foliage is attached to the ground.
We can sometimes fix this by multiplying the output from the Simple Grass Wind with the Blue output of a Bounding Box Based 0-1 UVW node.
Blue means vertical axis in this case. Not sure if in [[World Space]] or [[Model Space]].


# References

- [_How I Quickly Create 3D Environments in Unreal Engine 5 | FULL WORKFLOW_ - Wind, by pwnisher @ youtube.com](https://youtu.be/YZ4gSKZh6do?t=833)

