Spawning is the act of bringing an [[Actor]] into existence in a [[World]].
It is done with `UWorld::SpawnActor`.
`SpawnActor` takes two types.
The first is a template parameter that dictate the return value of the call.
The second is a [[UClass]], or [[TSubclassOf]], parameter to select the type to spawn.
The second type must be the same as, or a subclass of, the first type.

# Avoiding Collision

Actors have a [[Property]] named Spawn Collision Handling Method.
This Property controls what the engine should do when it is spawning an instance of that Actor and detects a collision with something already in the world.
Can be one of:
- Always Spawn, Ignore Collisions.
	- The Actor is spawned anyway, with an initial overlap.
- Try To Adjust Location, But Always Spawn.
	- The engine is allowed to pick a new spawn location if needed.
	- If that fails then the Actor is spawned anyway, with an initial overlap.
	- I don't know how the engine picks a new location.
	- I don't know where the Actor is spawned if the engine can't find a non-colliding location.
		- The initial location, I assume, but I  can also imaging the engine trying to find the location with the smallest possible overlap.
- Try To Adjust Location, Don't Spawn If Still Colliding.
	- Similar to Try To Adjust Location, But Always Spawn, but instead of spawning with an overlap if no suitable location is found with this setting the Actor is simply not spawned at all.
- Do Not Spawn.
	- Only spawn if the initial location is non-overlapping.
