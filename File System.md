File system operations are performed with the `FPlatformFileManager` class.
This has a bunch of functions for dealing with files and directories.
Access with `FPlatformFileManager::Get().GetPlatformFile()`.
For example:
```cpp
FString FileName = /* Something. */;
if (FPlatformFileMnager::Get().GetPlatformFile().FileExists(*FileName))
{
	// The file exists.
}
```


Another set of file related functions exists in `FFileHelper`.
Another set of file related functions exists in `FPaths`.

I [[Blueprint Visual Script]] we have access to
- Get Project Content Directory
- Get Project Directory
- Get Project File Path
- Get Project Save Directory

