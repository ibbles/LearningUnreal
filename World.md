
# Creating A Temporary World

For example for a [[Unit Test]].

Example [(1)](https://minifloppy.it/posts/2024/automated-testing-specs-ue5/)
```cpp
TWeakObjectPtr<UWorld> CreateTemporaryWorld(const FName& Name)
{
	if (GEngine != nullptr)
		return nullptr;

	UWorld* World = UWorld::CreateWorld(EWorldType::Game, false, Name, GetTransientPackage());
	if (World == nullptr)
		return nullptr;
	
	FWorldContext& WorldContext = GEngine->CreateNewWorldContext(EWorldType::Game);
	WorldContext.SetCurrentWorld(World);

	World->InitializeActorsForPlay(FURL());
	if (IsValid(World->GetWorldSettings()))
	{
		// Need to do this manually since world doesn't have a game mode.
		World->GetWorldSettings()->NotifyBeginPlay();
		World->GetWorldSettings()->NotifyMatchStarted();
	}
	World->BeginPlay();

	return MakeWeakObjectPtr(World);
}

void DestroyTemporaryWorld(TWeakObjectPtr<UWorld>& WeakWorld)
{
	if (GEngine == nullptr)
		return;

	UWorld* World = WeakWorld.Get();
	if (World == nullptr)
		return;

	World->BeginTearingDown();

	// Make sure to cleanup all actors immediately.
	// DestroyWorld doesn't do this and instead waits for GC to clear everything up.
	for (auto It = TActorIterator<AActor>(World); It; ++It)
	{
		It->Destroy();
	}

	GEngine->DestroyWorldContext(World);
	World->DestroyWorld(false);
}
```

I'm not sure all of the above is required in all cases.
It may be sufficient to do
```cpp
TObjectPtr<UWorld> World = UWorld::CreateWorld(EWorldType::Game, false, Name, GetTransientPackage());
// Do stuff.
World->DestroyWorld(false);
```


# References

- 1: [_Automated Testing With Specs in Unreal Engine_ by Francesco @ minifloppy.it 2024](https://minifloppy.it/posts/2024/automated-testing-specs-ue5/)
