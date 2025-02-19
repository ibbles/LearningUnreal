Consider a scenario where we have a collection of [[Asset]]s that references each other and we want to make a copy of that setup in order to make a new setup with some changes without altering the original assets.
We cannot simply copy each asset since the references must be updated, otherwise we get a bunch of new assets referencing the old assets which is not what we want.

# Advanced Copy

Unreal Editor has a feature called Advanced Copy.
In the Content Browser, select a bunch of assets and/or folders and drag them to a new folder. A menu appears. Select Advanced Copy Here.
This will update any references in the copied [[Asset]]s that points to one of the original [[Asset]]s to instead point to the newly created copy of that [[Asset]].
A limitation of this approach is that it cannot copy [[Level]]s.


# Migrate Assets

Another way that works with [[Level]]s is to use [[Asset Migration]] to migrate all the original assets to a separate, possibly temporary, project, open that project, move the [[Asset]]s to where we want them to be, and finally migrate them back to the original project into their new location.

The migration target project can be as simple as a folder containing an empty folder named Content and a `.uproject` file with the following content:
```json
{
       "FileVersion": 3,
       "EngineAssociation": "5.5",
       "Category": "",
       "Description": ""
}
```

Set the Engine Association field as appropriate.
Not sure if this makes any difference or not, but might as well.
