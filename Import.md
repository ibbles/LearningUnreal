Import is the act of getting any kind of media into the [[Unreal Editor]].
This can be anything that Unreal supports:
- [[Texture]]
- [[Sound]]
- [[Mesh]]
- [[Animation]]
- Combinations of the about in e.g. an FBX file.

We call the file that is being imported the source asset, and the result just an [[Asset]].
The source asset will remain in its original location on the file system.
During the import the Unreal Editor will convert the source format into a format suitable for Unreal.
It will also populate the [[Derived Data Cache]].
A reference to the source asset will be stored in the [[Asset]].
A dialog window with settings for the import and conversion may be displayed.

There are multiple ways to import a source asset into Unreal:
- Drag the source asset from a system file browser into the [[Content Browser]].
- Click the Import button in the [[Content Browser]].

Multiple source assets can be selected and dragged from a system file browser, or selected in the dialog opened when clicking the Import button.


# Auto Reimport

Content directories can be registered as being monitored,
meaning that any changes to the source assets in that directory will trigger a reimport.
This is controlled from [[Editor Preferences]] > General > Loading & Saving > Auto Reimport.
It looks like only directories inside the projects Content directory can be monitored.


# References

- [_Materials Master Learning_ > _Collaboration and Asset Pipeline_ by Epic Games @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/6r5/collaboration-and-asset-pipeline)
