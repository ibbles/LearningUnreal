A [[Level]] storage system where the data for the [[Actor]]s in the level is stored separate from the [[Level]]'s data.
The alternative is to store the [[Actor]]s' data inline in the [[Level]]'s data.
The purpose is to make collaboration easier.
When all data is in a single file then only one person at the time can work on a level.
With One File Per Actor multiple people can work on the same [[Level]] as long as they don't modify the same [[Actor]].

The files where the [[Actor]] data is stored is located at `$PROJECT_DIR/Content/__ExternalActors__/LEVEL_NAME/`.

One File Per Actor is enabled at [[World Settings]] > World > Use External Actors.
This is enabled by default for any [[Level]] created with [[World Partition]] enabled.

If you use a version control system that Unreal Editor has support for then you can enable [[Editor Preferences]] > General > Loading & Saving > Source Control > Automatically Checkout On Asset Modification.
This prevents other people from modifying an [[Actor]] you have modified,
and prevents you from modifying an [[Actor]] someone else has already modified.


# References

- [_Large worlds in UE5: A whole new (open) world | Unreal Engine_ by Epic Online Learning 2021 UE5.0](https://dev.epicgames.com/community/learning/talks-and-demos/KBe/large-worlds-in-ue5-a-whole-new-open-world-unreal-engine)
