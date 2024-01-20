Just like a C++ class member, a [[UObject]] [[Property]] can have different visibility levels.
Unreal Engine differentiates between default, or template, objects and instance objects.
A default, or template, object is like a C++ class, it is used to create, or instantiate, instances.
Examples of template objects include the [[Class Default Object]] and anything that lives inside a [[Blueprint Class]].
An instance is an object that exists in a world, something that is an actual thing in our game.
An instance is created from a template.
A template has a list of template instances that has been created from it.

A [[Property]] may be accessible in the [[Details Panel]] depending on if the selected object is a template or an instance.
When the [[Property]] is accessible it may be either visible or editable.
Editable includes visible, so it's read-only or read-write.
What is it depend on which of a family of [[Property Specifier|Property Specifiers]] we give it.
The patterns is a follows:
```
(Edit|Visible)(DefaultsOnly|InstanceOnly|Anywhere)
```

The first bit tells us if the [[Property]] is editable or just visible.
- `Edit` The property is editable.
- `Visible` The property's value is displayed, but cannot be edited.

The second bit tells us for what type of objects the [[Property]] is editable or visible.
- `DefaultsOnly` The [[Property]] is editable or visible on default, or template, objects, but not in instances.
- `InstanceOnly` The [[Property]] is editable or visible on instances only, but not on default objects.
- `Anywhere` The [[Property]] is editable or visible on both default objects and instances.

This gives us 2 * 3 = 6 combinations. They are:
```cpp
UPROPERTY(EditDefaultsOnly)
double ReadWriteInBlueprints_HiddenOnInstances;

UPROPERTY(VisibleDefaultsOnly)
double ReadOnlyInBlueprints_HiddenOnInstances;

UPROPERTY(EditInstanceOnly)
double HiddenInBlueprints_ReadWriteOnInstances;

UPROPERTY(VisibleInstanceOnly)
double HiddenInBlueprints_ReadOnlyOnInstances;

UPROPETY(EditAnywhere)
double ReadWriteInBlueprints_ReadWriteOnInstances;

UPROPERTY(VisibleAnywhere)
double ReadOnlyInBlueprints_ReadOnlyOnInstances;
```

From the [documentation](https://docs.unrealengine.com/4.26/en-US/ProgrammingAndScripting/GameplayArchitecture/Properties/Specifiers/) it doesn't seem possible to combine these.
Which means that there are a bunch of combinations that I don't know how to express.
Unless I'm misunderstanding something.

```
                          +---------------------------------------------------------------+
                          |                        Defaults                               |
                          | Read only           | Read write       | Hidden               |
+-------------------------+---------------------+------------------+----------------------+
|              Read only  | VisibleAnywhere     | ???              | VisibleInstanceOnly  |
|             ------------+---------------------+------------------+----------------------+
|  Instances   Read write |  ???                | EditAnywhere     | EditInstanceOnly     |
|             ------------+---------------------+------------------+----------------------+
|              Hidden     | VisibleDefaultsOnly | EditDefaultsOnly | (default)            |
+-------------------------+---------------------+------------------+----------------------+
```


# Dynamic Edit Condition Based On Other Properties

We can make a [[Property]] be either editable or not depending on the value of some other [[Property]].
This is called [[Edit Condition|`EditCondition`]] and is a [[Meta Property Specifier]].

Example:
```cpp
class MYPROJECT_API UMyObject : public UObject
{
	GENERATED_BODY()
public:
	UPROPERTY(EditAnywhere, Category = "My Object", Meta = (EditCondition = "bMyPropertyEditable"))
	double MyProperty = 0.0;

	UPROPERTY(EditAnywhere, Category = "Overrides", Meta = (PinHiddenByDefault, InlineEditConditionToggle))
	bool bMyPropertyEditable = false;
};
```

(
Experiment with the above, see what each part does and if all of them are necessary.
)

The `EditCondition` can be a more complicated expression, with `!`, `&&`, and `||`.
Not sure if function calls are allowed.

If there are many such Booleans then they may be merged into a bitfield:
```cpp
UPROPERTY(...)
uint8 bFirstPropertyEditable : 1 = false;
UPROPERTY(...)
uint8 bSecondPropertyEditable : 1 = false;
UPROPERTY(...)
uint8 bThirdPropertyEditable : 1 = false;
// And so on.
// Inline initialization of bit field members may require C++20,
// initialize them in the constructor if you have an older C++ compiler.
```

[_FYI, I just discovered how to make an "override" checkbox in C++, like the ones you see on PostProcessVolume and other built-in components._ by heyheyhey27 @ reddit.com. 2019](https://www.reddit.com/r/unrealengine/comments/ctbiy8/fyi_i_just_discovered_how_to_make_an_override/)
