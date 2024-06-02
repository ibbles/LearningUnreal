
A type of [[Actor]] that brings in an entire [[Level]] into another.
Create from the Place Actors panel.
Has [[Details Panel]] > Level Instance > World Asset which is a reference to a [[Level]] [[Asset]].
All the [[Actor]]s that the [[Level]] [[Asset]] contains will be placed around the Level Instance [[Actor]].

The Level Instance can be realized in different ways during runtime:
- Embedded: The Level Instance is just a modeling tool. At runtime the constituent [[Actor]]s are created in the enclosing level as-if they had been created there directly by hand.
- Level Streaming: The Level Instance is streamed like a regular level.


See also [[Packed Level Instance]].

# References

- [_Large worlds in UE5: A whole new (open) world | Unreal Engine_ by Epic Online Learning 2021 UE5.0](https://dev.epicgames.com/community/learning/talks-and-demos/KBe/large-worlds-in-ue5-a-whole-new-open-world-unreal-engine)

