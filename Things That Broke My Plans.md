This note is about things I didn't know about Unreal Engine that ruined a design idea I had but was forced to scrap.


# Cannot Call Enable Input On A Pawn

Sometimes I want to control multiple [[Actor]]s at once.
I typically do this by having the [[Player Controller]] call [[Enable Input]] and Disable Input on the [[Actor]]s according to input from the player.
This does not work when the [[Actor]] is a [[Pawn]]:
```
LogPawn: Error: EnableInput can only be specified on a Pawn for its Controller
```
I'm not even sure what that sentence means, but is is clearly an error.
I understand that the intention is that a [[Player Controller]] should control a [[Pawn]],
but the [[Player Controller]] cannot possess multiple [[Pawn]]s.
So I want to [[Enable Input]] to the "extra" [[Pawn]].
I don't know how to solve this.


# Cannot Have A Pointer To An Actor Component

It is not possible to have a pointer to an [[Actor Component]].
This is because of [[Blueprint Reconstruction]].
At any point an [[Actor]] may have all its [[Actor Component]]s destroyed and recreated.
If you have a pointer to such a destroyed Component then bad things will happen.

Also, there is no picker for [[Actor Component]] [[Property]]s like there is for [[Actor]] [[[Property]]s.

Together these two quirks of the Unreal Engine makes it very difficult to have one [[Actor]] reference an [[Actor Component]] in another [[Actor]].
An endeavor that is made even more difficult due to _Sublevels And Pointer To Actor_.


# Sublevel And Pointer To Actor

Some [[Actor]] types need a reference to another [[Actor]], such as a button opening a door.
A [[Level]] can be split into [[Sub Level]]s.
An [[Actor]] in one [[Sub Level]] cannot have a pointer to an [[Actor]] in another [[Sub Level]], even if both [[Sub Level]]s have their [[Streaming Method]] set to Always Loaded.

