Import is the act to getting any kind of media into the [[Unreal Editor]].
This can be anything that Unreal supports:
- [[Texture]]
- [[Sound]]
- [[Mesh]]
- [[Animation]]
- Combinations of the about in e.g. an FBX file.

We call the file that is being imported the source asset, and the result just an asset.
The source asset will remain in its original location on the file system.
During the import the Unreal Editor will convert the source format into a format suitable for Unreal.
It will also populate the [[Derived Data Cache]].
A dialog window with settings for the import and conversion may be displayed.

There are multiple ways to import a source asset into Unreal:
- Drag the asset from a system file browser into the [[Content Browser]].
- Click the Import button in the [[Content Browser]].

# Auto Reimport

Content directories can be registered as being monitored,
meaning that any changes to the source assets in that directory will trigger a reimport.
This is controlled from [[Editor Preferences]] > General > Loading & Saving > Auto Reimport.
It looks like only directories inside the projects Content directory can be monitored.


# References

- [_Materials Master Learning_ > _Collaboration and Asset Pipeline_ by Epic Games @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/6r5/collaboration-and-asset-pipeline)
