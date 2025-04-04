A Player Controller is a type of [[Controller]].
The Player Controller is part of the [[Pawn]], Player Controller, [[Player State]] trio created for each player.

The Player Controller may be possessing, i.e. controlling, a [[Pawn]].
We can get access to that [[Pawn]] with the Get Controlled Pawn node.
In C++ we call the `AController::GetPawn` function.
We can check if a particular [[Pawn]] is controlled by a Player Controller with the Is Player Controlled node.
Inside a [[Pawn]] we get find the current Player Controller with the Get Controller node and a [[Cast|cast]].
We can get a Player Controller from anywhere with the [[Get Player Controller]] node,
and the `UGameplayStatics::GetPlayerController` C++ function..
In that case we must know the index of the Player Controller we want.

The Player Controller translates user input into movement, inventory navigation, or interaction with something near-by in the game world.

A Player Controller has a [[HUD]] that we can access with the Get HUD node.

The [[Player Controller]] replicates from the client to the server,
but not to other clients.
Use [[Player State]] for per-player data that needs to be known by other clients.

As of Unreal Engine 5.1 the default input system is [[Enhanced Input]].


# Input Events

The Player Controller will receive [[Input Event]] generated by the [[Input Bindings]] configured at [[Project Settings]] > Engine > Input > Bindings.

# Mouse Cursor

The Player Controller is in control of the [[Mouse]] cursor.


## Mouse Interface

This is a category in [[Class Defaults]] > [[Details Panel]].

- Show Mouse Cursor.
	- Controls if the mouse cursor should be rendered or not.
- Enable Click Events [(2)](https://youtu.be/G9ak4aFfJUE?t=769).
	- Controls if clicks should do ray-casting into the world and call [[Event Actor On Clicked]] on the hit [[Actor]].

## Get Hit Result Under Cursor By Channel

A Player Controller has access to the Get Hit Result Under Cursor By Channel function.
This uses the mouse cursor to do a line trace into the scene.
From the Hit Result we get a Location, which is in world space.


# References

- 1: [_Begin Play | Gameplay_ by Epic Games, Samuel Bass @ dev.epicgames.com 2022](https://dev.epicgames.com/community/learning/tutorials/l21z/unreal-engine-begin-play-gameplay) 
- 2: [_Unreal Engine 5 Tutorial - Action RPG Part 2: Interactions_ by Ryan Laley @ youtube.com 2023](https://youtu.be/G9ak4aFfJUE?t=769)
