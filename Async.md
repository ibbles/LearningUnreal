Run a task on a specific thread at some point in the future with `AsyncTask` [(1)](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Core/Async/AsyncTask).

Example deferring some work to be performed on the [[Game Thread]]:
```
AsyncTask(ENamedThreads::GameThread, [this]()
{
    GEngine->AddOnScreenDebugMessage(-1, 5.f, FColor::Green, TEXT("Running on the game thread."));
});
```

# References

- 1: [_AsyncTask_ @ dev.epicgames.com/documentation UE 5.5](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Core/Async/AsyncTask)
- 2: [_Task System_ @ dev.epicgames.com/documentation UE 5.5](https://dev.epicgames.com/documentation/en-us/unreal-engine/tasks-systems-in-unreal-engine)
