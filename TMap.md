
The following uses `TMap<int, double> Map` as an example.
Other names are used as well, where there are more instructive names.

A map consists of elements and each element contains a key and a value.
In the example map, the keys are `int` and the values are `double`.


# Populating A `TMap`

Add elements with `Map.Add(1, 1.0)`.
```
1 → 1.0
```

Add a default-constructed value with `Map.Add(0)`.
The default value for a 'double' is 0.0.
(
Is a primitive-typed value default-initialized or value-initialized?
I really hope value-initialized.
)
```
1 → 1.0
0 → 0.0
```

Avoid temporaries with `Map.Emplace(2, 2.0)`.
```
1 → 1.0
0 → 0.0
```
Doesn't matter much for primitive types, but can be important for larger composite types.

Add all elements from one map to another with `DestinationMap.Append(SourceMap)`.


# Querying A `TMap`

Get reference to the value for a key you know for sure exists with `Map[1]`.

Check if a map contains a key with `Map.Contains[1]`.
If the `Contains` call is followed by a second look-up with the same key then there is usually a better way to do it that doesn't require two look-ups.

Search for a particular key with `Map.Find(1)`.
Returns a pointer to the value if it exists and `nullptr` if not.

Add an element if the key doesn't exist with `Map.FindOrAdd(3)`.


Get a default constructed value if the key doesn't exist with `Map.FindRef(4)`, this returns 0.
Do a reverse lookup, i.e. scan for a value, with `Map.FindKey(2.0)`, this returns 2.


# Iterating Over A `TMap`

Loop over elements with `for (auto& Elem : Map) { Elem.Key; Elem.Value }`.


# Depoulating A `TMap`

Remove an element with `Map.Remove(1)`.



(
TODO There are more functions.
)

# References

- [_TMap_ by Epic Games @ docs.epicgames.com/documentation](https://dev.epicgames.com/documentation/en-us/unreal-engine/map-containers-in-unreal-engine)
