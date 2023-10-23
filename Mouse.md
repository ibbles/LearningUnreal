In a [[Player Controller]] we can find the current location, in world space, of the mouse cursor using `DeprojectMousePositionToWorld`.
It takes two output parameters:
- `FVector WorldLocation`
- `FVector WorldDirection

The World Location is a location that is hit when tracing a ray from the camera through the mouse cursor location on the near clip plan and into the world.
I don't know the engine does an actual intersection test against geometry in the world, or if we get a default distance from the camera.

The World Direction is the direction of the ray.

```cpp
FVector WorldLocation;
FVector WorldDirection;
DeprojectMousePositionToWorld(
	WorldLocation, WorldDirection);
```

