This note describe how to set per-instance default values during [[Construction Script]] that can still be modified from the [[Details Panel]].
By default any value that is modified during instance constructions is considered a [[User Construction Script Modified Property]] and becomes read-only in the [[Details Panel]].

Unreal Engine has a concept called [[Construction Script]].
This is a piece of [[Blueprint Visual Script]] or C++ code that is run when an object is being created.
The following C++ functions are considered part of the [[Construction Script]]:
- Post Init Properties
- Post Load

A difference between a C++ constructor and the Construction Script is that the [[Property|Properties]] edited during the constructor,
at least the [[Class Default Object]] constructor, are part of the baseline state of the object while properties edited during the [[Construction Script]] are special.
A [[Property]] modified during the [[Construction Script]] is considered a derived property,
meaning that its value may depend on other values in the object.
That is, the [[Construction Script]] may read user-configurable [[Property|Properties]] and set the derived property based on that.
In the normal case it wouldn't make sense for the user to modify such a [[Property]] since it would get overwritten by the construction script immediately, which is why it becomes grayed out in the [[Details Panel]].

We can tell Unreal Engine that a [[Property]] should remain editable even af

Consider an example Component type named Main Component.
Call order during Unreal Editor startup:
- Constructor for [[Class Default Object]].
- Post Init Properties for [[Class Default Object]].
- Constructor for instance in level.
- Post Init Properties for instance in level.
- Serialization data read for instance in level.
- Post Load for instance in level.

Any write that happens in Post Load will overwrite data stored in the serialization data.

Writes made in the constructor and Post Init Properties are considered the canonical state of the type.
These values will not get a reset arrow button next to them in the [[Blueprint Class Editor]] [[Details Panel]].
Writes made in Post Load are not considered the canonical state of the type.
These values will get a reset arrow button next to them in the [[Blueprint Class Editor]] [[Details Panel]].

When opening a [[Blueprint Class]] containing the example Component the following happens:
- An instance with the `_GEN_VARIABLE` name suffix is created, Post Init Properties, Post Load is called.
- An instance with the user-provided name is created, Post Init Properties, Post Load is called.

Changes made to a [[Property]] in a Blueprint [[Construction Script]] is not visible in the [[Blueprint Class Editor]] [[Details Panel]].
The new values are visible in the [[Details Panel]] of an instance of the [[Blueprint Class]] in the level.
The value is not grayed out and appear to be editable, but are reset back to the Blueprint Construction Script value immediately.
Unless the [[Property]] has been marked with the Skip UCS Modified Properties Meta flag.
In that case the value can be edited and the changes stick.
Clicking the reset arrow button next to the [[Property]] resets it to the [[Construction Script]] value.

Edits made to a [[Property]] that is modified in Post Load from the [[Blueprint Class Editor]] [[Details Panel]] will be reset when Unreal Editor is restarted.
Simply closing and reopening the [[Blueprint Class Editor]] is not enough for Post Load to be run again.

Editing an instance of a [[Blueprint Class]] in the level and then closing and reopening Unreal Editor has the following effect:
- Set in constructor, not Skip UCS: Edited value **retained**.
- Set in Post Init Properties, not Skip UCS: Edited value **retained**.
- Set in Post Load, not Skip UCS: Edited value **overwritten** by Post Load.
- Set in [[Blueprint Visual Script]] [[Construction Script]], not Skip UCS: Not editable in the first place.
- Set in constructor, with Skip UCS: Edited value **retained**.
- Set in Post Init Properties, with Skip UCS: Edited value **retained**.
- Set in Post Load, with Skip UCS: Edited value **overwritten** by Post Load.
- Set in [[Blueprint Visual Script]] [[Construction Script]], with Skip UCS: Edited value **retained**.

# References

- https://udn.unrealengine.com/s/question/0D52L00005KLF5ZSAX/how-can-i-provide-default-values-for-a-uproperty-outside-of-the-constructor  
* https://udn.unrealengine.com/s/question/0D52L00004luoN5SAI/setting-a-value-on-a-component-property-when-actor-placed-in-the-level-editor  
* https://udn.unrealengine.com/s/question/0D54z00009itP8yCAE/postprocess-volume-corrupted-in-53

