Every object that has a place in the world has a transformation.
The transformation describe an object's location, rotation, and scale.
These act in the Unreal Engine [[Coordinate System]].
Transformations can be chained, one being applied on the result of the one before it.
We call this a **transformation hierarchy**, since each step can branch into many child transforms.
The Scene Components of an [[Actor]] is an example of a transformation hierarchy.
Actors attached to each other is another example of a transformation hierarchy.
We call each level in the hierarchy a coordinate frame, frame, or a space.
Each transform can be seen as a local transformation or a global transformation.
The **local transformation** tells you where the transformed object is relative to the transformation's parent transformation.
Also called the local space of the parent.
The **global transformation** tells you where the transformed object is relative to the world.
Also called world space or the world coordinate system.
The **world** is the coordinate system you end up in if you walk up the parent chain until there are no more parents.

Each object has a point that defines its **origin**.
An object that has the **identity transformation**, i.e. a no-op transformation, is placed with its origin at the origin of the parent frame.
An Actor's origin is marked by a white sphere icon.
If you ask for an [[Actor]]'s location then you will get the position of the Actor's [[Root Component]] origin in world space.


# Transformation Operations

We sometimes need to do operations on one or more transformations.
This is often difficult to get right.

## Finding A Point In One Space When Known In Another

Use `FTransform::GetRelativeTransform`.

Consider a case where you have a point known in some space and you need to know what the coordinate of that point is in some other space.
For example, consider a gameplay object that has a target on it and that object knows where on itself the target is.
That is, the object knows the location of the target within the object's local space.
That is, no matter how the object is moved in the world the local location of the target remain fixed as long as the target remain attached to the object.
Also consider another object that needs to aim at the target.
It would be useful for that other object to know where the target is relative to itself,
so that it knows how it should turn and tilt to aim at the target.
So we know Target Relative To Owner,
and we want to know Target Relative To Other.
We can use `FTransform::GetRelativeTransform` to compute Target Relative To Other.
```cpp
FVector LocateTargetRelativeToOther(
	const FTransform& Owner,
	const FVector& TargetInOwner,
	const FTransform& Other)
{
	// Create a transformation that converts points in the Owner space
	// to the equivalent point in the Other space.
	const FTransform OwnerToOther = Owner.GetRelativeTransform(Other);
	const FVector TargetInOther = OwnerToOther.TransformPosition(TargetInOwner);

	const FVector TargetInWorldAccordingToOwner = Owner.TransformPosition(TargetInOwner);
	const FVector TargetInWorldAccordingToOther = Other.TransformPosition(TargetInOther);
	assertEqual(
		TargetInWorldAccordingToOwner,
		TargetInWorldAccordingToOther);

	return TargetInOther;
}

// Example usage with all made-up classes and functions.
// Both UCannon and UTargetsOwner are subclasses of USceneComponent.
void UCannon::AimAtTarget(UTargetsOwner& TargetOwner, int32 TargetIndex)
{
	const FVector TargetInOwner = TargetOwner.GetTargetLocalLocation(TargetIndex);
	const FVector TargetInSelf = LocateTargetRelativeToOther(
		TargetOwner.GetComponentTransform(),
		TargetInOwner,
		GetComponentTransform());
	// Turn and pitch the turrent so that it aligns with TargetInSelf.
}
```



# Helpful Functions

There may be more useful stuff in [[Blueprint Utility And Math Functions And Techniques]].

- Find Look At Rotation(`Vector Start`, `Vector Target`) → `Rotator`
  - Return the [[Rotator]] that rotates `Start` to `Target`.
- [[Actor]] > Get World Location
  - Return the location in world space of the [[Root Component]] of the [[Actor]].
- Restrict a [[Rotator]] to only only axis by splitting it and only passing one of Roll, Pitch, Yaw to a Make Rotator node.
- We can call Get Forward Vector, Get Right Vector, and Get Up Vector on many things.
	- Returns the requested vector in the object's local space, expressed in the world coordinate frame.



