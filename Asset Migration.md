[[Asset|Assets]], and all their dependencies, can be migrated from one Unreal Engine project to another.
Select the asset to migrate in the [[Content Browser]]]] and right-click > Asset Actions > Migrate.
The editor will find everything referenced by the selected asset and a list of everything that will be migrated is shown.
Click OK and a directory selection dialog is opened.
Select the Content directory of the Unreal Engine project to which the assets should be migrated.

A migrate doesn't do anything special, other than following references.
The migration step is just a file copy.
You can do the file copy yourself using any file system copy tool.
Though beware that you should make sure everything that the asset needs is included,
and you must copy to the exact same directory hierarchy in the destination project.
If an [[Asset]] was in `Content/MyProject/Buildings/Shed/Textures` in the source project then it must be in the same directory in the destination project for references to that Asset to work.

A recommendation is to make folders self-contained.
Any asset with references to other assets should not point to widely different folders.
Any particular object should live in a folder and a copy of that folder should contain every asset that object needs.
Do:
```
Content/
	MyProject\
		Buildings/
			Shed/
				Textures/
				StaticMeshes\
				Materials\
```
Don't:
```
Content/
	Textures/
		Buildings/
			Shed/
	StaticMeshes/
		Buildings/
			Shed/
	Materials/
		Buildings/
			Shed/
```
