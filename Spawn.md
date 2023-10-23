Spawning is the act of bringing an [[Actor]] into existence in a [[World]].
It is done with `UWorld::SpawnActor`.
`SpawnActor` takes two types.
The first is a template parameter that dictate the return value of the call.
The second is a [[UClass]], or [[TSubclassOf]], parameter to select the type to spawn.
The second type must be the same as, or a subclass of, the first type.
