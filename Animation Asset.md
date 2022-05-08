An Animation Asset is an asset that contains animation data for a [[Skeleton Asset]].
Animations are often created in a third-party software and imported into Unreal Editor.
Often as FBX files.
Animations are **imported** with Content Browser >  Import.
When importing an animation you must select a [[Skeleton Asset]] that has a bone structure matching the animation.
FBX files can contain both animation and mesh data.
Disable Import Mesh in the FBX Import Options if you already have a [[Skeletal Mesh Asset]] for your [[Skeleton Asset]].
When imported the we get the animation as an **Animation Montage**.

The Animation Editor contains
- a Viewport where we can see the animation.
- a Details panel.
- an Asset Browser.
  Shows a list of all animations available for the [[Skeleton Asset]].
- a timeline that can be used to view different points in time of the animation in the Viewport.

Which animation to play at any point in time is controlled with an [[Animation Blueprint]].
