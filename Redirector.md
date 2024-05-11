A redirector is a hidden [[Asset]] that points to another asset.
It is created by [[Unreal Editor]] when we use the [[Content Browser]] to move an asset from one place to another.
A redirector will be placed at the old location so that any other asset that references the moved asset will be able to find it at the new location.
To make the other asset look at the no location instead  of going via the redirector, right-click the folder containing the redirector (Not sure about this one. The folder with the redirector, or the folder with the reference?) and select Fix Up Redirectors.
The redirectors can be unhidden with [[Content Browser]] > Filter > Redirectors.


# References

- [_Becoming an Environment Artist in Unreal Engine_ > _Setting Up Your Folders_ by Epic Games 2020 UE4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/pvY/unreal-engine-setting-up-your-folders)
