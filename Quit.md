Quit a game by running the `Quit` [[Console Command]].

One way to run a [[Console Command]] is to somehow get a hold of a [[Player Controller]].
Player Controller has the Console Command member function.
```cpp
APlayerController* Controller = /* Get the Player Controller somehow. */

Controller->ConsoleCommand("Quit");
```

A Player Controller can be fetched from:
- A [[HUD]] using Player Owner.
