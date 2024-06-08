
A [[Level]] storage system where the data for the [[Actor]]s in the level is stored separate from the [[Level]]'s data.
The alternative is to store the [[Actor]]s' data inline in the [[Level]]'s data the `.umap` file.
The purpose of One File Per Actor is to make collaboration easier when multiple people are working on the same [[Level]].
More work can be done in parallel without source control collisions.
When all data is in a single `.umap` file then only one person at the time can work on a level.
With One File Per Actor multiple people can work on the same [[Level]] as long as they don't modify the same [[Actor]].
When there is a collision the collision is limited to individual [[Actor]]s,
which often makes it easier to fix.

The files where the [[Actor]] data is stored is located at `$PROJECT_DIR/Content/__ExternalActors__/LEVEL_NAME/`.

One File Per Actor is enabled at [[Editor Preferences]] > General > Experimental > Tools > Enable One File Per Actor.
One File Per Actor is enabled at [[World Settings]] > World > Use External Actors.
This is enabled by default for any [[Level]] created with [[World Partition]] enabled.
One File Per Actor is controlled per Actor at [[Details Panel]] > Advanced > Packaging Mode.
(This may have changed since Unreal Engine 5.0.)

After working for a while you will have a bunch of changed files to commit to [[Source Control]].
In-editor [[Source Control]] can help you commit related changes together and separate from other changes.
Commit often.

If the [[Source Control]] system marks a file you don't know what it is as modified copy the file name, without `.uasset` into the [[Outliner]] search bar and the [[Actor]] that the file belongs to will be shown.

If you use a version control system that Unreal Editor has support for then you can enable [[Editor Preferences]] > General > Loading & Saving > Source Control > Automatically Checkout On Asset Modification.
This prevents other people from modifying an [[Actor]] you have modified,
and prevents you from modifying an [[Actor]] someone else has already modified.


# References

- [_Large worlds in UE5: A whole new (open) world | Unreal Engine_ by Epic Online Learning 2021 UE5.0](https://dev.epicgames.com/community/learning/talks-and-demos/KBe/large-worlds-in-ue5-a-whole-new-open-world-unreal-engine)
- [_Building Open Worlds in Unreal Engine 5 | Unreal Fest 2022_ by Epic Online Learning @ dev.epicgames.com/talks-and-demos](https://dev.epicgames.com/community/learning/talks-and-demos/LLM5/building-open-worlds-in-unreal-engine-5-unreal-fest-2022)
- [_Building Bigger: Changing Your Workflow for Building Worlds instead of Scenes | Unreal Fest 2023_ by Chris Murphy @ dev.epicgames.com/talks-and-demos 2024 UE5.3](https://dev.epicgames.com/community/learning/talks-and-demos/jwlJ/unreal-engine-building-bigger-changing-your-workflow-for-building-worlds-instead-of-scenes-unreal-fest-2023)


