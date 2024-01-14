A Gameplay Tag Container is a type we can add as a variable to our Blueprint (and C++?) classes.
It holds a collection of [[Gameplay Tag|Gameplay Tags]], which are hierarchical, named, boolean flags that can be enabled or disabled.
The Gameplay Tag Container has a source which is an `.ini` file that lists all the [[Gameplay Tag|Gameplay Tags]].
This makes it possible to have the same set of tags in many classes.

The Gameplay Tag Container has the Has Tag member function.
It returns true if the given tag is enabled in the Gameplay Tag Container.
It has an Exact Match parameter but I don't understand that one yet, keep it set to false for now.

The Gameplay Tag Container has the Add Gameplay Tag member function.
This enables the given tag in the Gameplay Tag Container.
I assume the tag must be supported by this particular Gameplay Tag Container,
i.e. is must be listed in the source used by this particular Gameplay Tag Container.

I assume there is a Remove Gameplay Tag member function as well,
that disabled the given Gameplay Tag.


# References

- [_USE Gameplay Tags_ by The Game Dev Cave @ youtube.com 2024](https://www.youtube.com/watch?v=1T4S2lFf19s)
