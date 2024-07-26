Unreal Engine contains a pointer library similar to that introduced in C++11.
They are for "regular" objects only, things allocated and deleted manually.
They is not for `UObject` types, they use a separate memory-tracking system based on [[Garbage Collection]].

Smart pointers prevent memory leaks by automatically deleting objects that they point to at the appropriate time.
Typically when the (last) smart pointer is destructed.
Smart pointers convey semantics through the type system.
A shared pointer signals shared ownership.
A unique pointer signals single ownership.
A reference signals non-nullness.
Smart pointer should typically not be passed as parameters, only if the called function affect ownership in some way.
Better to pass the actual object as `const &`.
Smart pointers can point to incomplete types, helps reduce header inclusion.

The smart pointers include
- `TSharedPtr`: Pointer that keeps the pointed-to object alive.
- `TSharedRef`: Like `TSharedPointer` but cannot be `nullptr`.
- `TWeakPtr`: Non-owning pointer.
- `TUniquePtr`: A single owner only. Object is deleted when the `TUniquePtr` is destroyed.


# `TSharedPointer`

Keeps the object it points to alive, it will not be automatically deleted as long as at least one `TSharedPtr` (or `TSharedRef`) points to it.
The last `TSharedPtr` (or `TSharedRef`) pointing to an object will delete it.
Can be `nullptr`.
Can be converted to a `TSharedRef`, if not `nullptr`.
Cannot point to an object that a `TUniquePtr` points to.


# `TSharedRef`

Like a `TSharedPtr`, but cannot be `nullptr`.
Keeps the object it references alive, it will not be automatically deleted as long as at least one `TSharedRef` (or `TSharedPtr`) references it.
The last `TSharedRef` (or `TSharedPtr`) pointing to an object will delete it.
Can always be converted to a `TSharedPtr`, which will not be `nullptr`.
Cannot point to an object that a `TUniquePtr` points to.


# `TWeakPtr`

Points to an object but doesn't keep it alive.
Used to break cycles that would create undeletable object groups if both/all were `TSharedPtr`.
Make one of them `TWeakPtr` to break the cycle.
Becomes `nullptr` when the pointed-to object is deleted.
To access the pointed-to object create a `TSharedPtr` from the `TWeakPtr`.
That way you know that the object will be kept alive for as long as you use it.
Cannot point to an object that a `TUniquePtr` points to.


# `TUniquePtr`

A single owner of an object.
There cannot be multiple `TUniquePtr` pointing to the same object.
Other (raw) pointers may point to it, but the single owning `TUniquePtr` decides when the object is deleted.
Typically when the `TUniqePtr` destructor is run.
The object can be transferred from one `TUniquePtr` to another.
Cannot point to an object that a `TSharedPtr` points to or a `TSharedRef` references.


# Helper Utilities

`TSharedFromThis` is an optional base class that makes it possible to create `TSharedRef`s from instances of the inheriting class.
Use the `AsShared` and `SharedThis` member functions.

`MakeShared` allocates a new object and creates a `TSharedRef` to it.
`MakeSharable` is used to make a `TSharedRef` or `TSharedPtr` from a non-owned raw pointer.
It creates a temporary object that can be assigned to a `TSharedRef` or a `TSharedPtr`.

`StaticCastSharedRef` and `StaticCastSharedPtr` are like the regular `Cast`, but for pointers wrapped in a smart pointer.
`ConstCastSharedRef` and `ConstCastSharedPtr` are like the regular `const_cast`, but for pointers wrapped in a smart pointer.




# References
- [_Unreal Smart Pointer Library_ by Epic Games @ docs.unrealengine.com. UE 5.0.](https://docs.unrealengine.com/5.0/en-US/smart-pointers-in-unreal-engine/)
- [_C++ For Unreal Engine (Part 2) | Learn C++ For Unreal Engine | C++ Tutorial For Unreal Engine_ by Nerd's Lesson, A.T. Chamillard @ youtube.com, 2022, UE4.27](https://youtu.be/IYJwU-rB2jA?t=13982)

