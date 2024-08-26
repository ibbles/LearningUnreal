We can't have a pointer to an [[Actor Component]] because they are transient,
that is, a Component may be destroyed and recreated at a new address at any time due to [[Blueprint Reconstruction]].
Any pointer to the old Component will become garbage.

There is `FComponentReference` that is kind of a reference to a Component, but it doesn't seem to work very well.
Not used much by engine code so it breaks from time to time, most recently in Unreal Engine versions 5.1 to 5.4.

I don't know how to properly reference a Component.

# References

- [_FComponentReference loses its actor reference when the target actor is reinstanced_ by Epic Games at issues.unrealengine.com](https://issues.unrealengine.com/issue/UE-185943)
