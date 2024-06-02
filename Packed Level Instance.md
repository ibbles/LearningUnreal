A type of [[Level Instance]]. (I think.)

(
Not sure what the difference between a Level Instance and a Packed Level Instance is.
)

A type of prefab used to group ??? ([[Actor]] or [[Component]]?) together, with the purpose of being placed repeatedly across the [[Level]].

Has a reference to a [[Blueprint Class]] and a World Asset.
Not sure what the [[Blueprint Class]] is used for.
A World Asset is a [[Level]] (I think.) that contains [[Actor]]s that are instantiated for every instance of the Level Instance.

If the World Asset is modified then we need to right-click any instance in the [[Outliner]] and select Update Packed Blueprint.
That will update all instances of the Level Instance in the [[Level]].

If you want to modify a particular instance of a Level Instance then you can right-click > Level Instance > Break > Break Level Instance.
This will take all the [[Actor]]s in the Level Instance and create them as regular free-standing [[Actor]]s.

# References

- [_Large worlds in UE5: A whole new (open) world | Unreal Engine_ by Epic Online Learning 2021 UE5.0](https://dev.epicgames.com/community/learning/talks-and-demos/KBe/large-worlds-in-ue5-a-whole-new-open-world-unreal-engine)

