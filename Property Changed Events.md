When a [[Property]] is edited in Unreal Editor the edited instance is notified about it through a pair of virtual member functions.
- `PostEditChangeProperty`
- `PostEditChangeChainProperty`

`PostEditChangeProperty` is given the Property on the instance that was changed, and also the leaf Property in case a `struct` is involved.
`PostEditChangeChainProperty` gives you the entire chain through any number of nested `structs`.

As usual, things get a bit buggy when Blueprints are involved.
One would expect that if multiple instances are selected and edited then the callbacks would be called on each edited instance.
When editing instances that have been created directly in a level, i.e. that are not part of a Blueprint, then this is exactly what happens.
