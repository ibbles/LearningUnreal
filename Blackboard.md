A Blackboard is a place for an [[AI Controller]] (Or is it a [[Behavior Tree]]?) to store data.
In the [[Behavior Tree]] editor set [[Details Panel]] > AI > Behavior Tree > Blackboard Asset to the Blackboard that contains the data this [[Behavior Tree]] needs.

A Blackboard contains a set of keys.
Create a new key with the New Key button in the top left of the Blackboard editor.
Each key has a name.
Select a key in the Blackboard panel to display the settings for that key in the Blackboard Details panel.

Each key has an associated value.
Set the type of the values with Blackboard Details panel > Key > Key Type.
If Key type is set to Object then the Base Class property is unhidden and can be set to specify the type of object that the value can reference [(1)](https://youtu.be/heoPNDwN57k?list=PLiDon2C0wI4tadc-85kLcDykqG4j6xIyc&t=247).

# Set Value From Game Logic

To set the value of a key during game-play use one of the Blackboard > Set Value functions [(1)](https://youtu.be/heoPNDwN57k?list=PLiDon2C0wI4tadc-85kLcDykqG4j6xIyc&t=466).
Here is an example of how to set the value of an Object-typed key named Target:

![Set value from Pawn](./Blackboard/set_value_from_pawn.jpg)

A value already set can be cleared with the Clear Value function.


# References

- 1: [_Unreal Engine 5 Tutorial - Action RPG Part 13: Enemy AI_ by Ryan Laley @ youtube.com 2024](https://youtu.be/heoPNDwN57k?list=PLiDon2C0wI4tadc-85kLcDykqG4j6xIyc)
