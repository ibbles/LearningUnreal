A Vector is a three-component value, holding floating-point numbers.
In Unreal Engine 4 the numbers are single-precision, 32-bit.
In Unreal Engine 5 the numbers are double-precision, 64-bit.

In a [[Blueprint Visual Script]] we can create a Vector using the **Make Vector** node.
There are also many nodes that get, compute, or manipulate Vectors.

On an [[Actor]] you can call **Get Actor Location** to get the Vector pointing from the world origin to the [[Actor]].
(
Is Get Actor Location really what the node is called? Or is it Get World Location?
)


On a Vector you can call **Find Look At Rotation** and pass in a Target to get the [[Rotator]] that rotates the start Vector to the Target Vector.
