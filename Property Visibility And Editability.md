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