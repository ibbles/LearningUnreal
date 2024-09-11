
The following uses `TMap<int, double> Map` as an example.
Other names are used as well, where there are more instructive names.

A map consists of elements and each element contains a key and a value.
In the example map, the keys are `int` and the values are `double`.

Add elements with `Map.Add(1, 1.0)`.
Add a default-constructed value with `Map.Add(0)`.
Avoid temporaries with `Map.Emplace(2, 2.0)`.
Add all elements from one map to another with `DestinationMap.Append(SourceMap)`.
Loop over elements with `for (auto& Elem : Map) { Elem.Key; Elem.Value }`.
Get reference to the value for a key you know for sure exists with `Map[1]`.
Check if a map contains a key with `Map.Contains[1]`.
Search for a particular key with `Map.Find(1)`. Returns a pointer to the value.
Add an element if the key doesn't exist with `Map.FindOrAdd(3)`.
Get a default constructed value if the key doesn't exist with `Map.FindRef(4)`, this returns 0.
Do a reverse lookup, i.e. scan for a value, with `Map.FindKey(2.0)`, this returns 2.
Remove an element with `Map.Remove(1)`.

(
TODO There are more functions.
)

# References

- [_TMap_ by Epic Games @ docs.epicgames.com/documentation](https://dev.epicgames.com/documentation/en-us/unreal-engine/map-containers-in-unreal-engine)
