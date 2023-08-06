Git is a version control system, meaning that it keeps track of the history of a project.
We can give names to incremental pieces of work, share that work with other people, and revert older versions if we mess up.
Git is designed to wok with text files and doesn't handle large binary files very well out of the box.
Use Git LFS to improve the situation somewhat, though we still don't get merge conflict resolution for binary files.


# Installing Git

These instructions are for Ubuntu, adapt as necessary for other platforms.
```shell
$ sudo apt install git git-lfs
$ git lfs install
```


# Creating A Repository

Git works with repositories.
A repository is a file system directory containing files tracked by Git.
It is common to let the root directory of an Unreal Engine project be a Git repository.
Or alternatively a parent directory that in addition to the project directory also contains other related material.


## Initialization

A Git repository is initialized with
```
/UnrealProjects/MyProject
$ git init .
```

It is safe to do this with files already in the directory.
Initializing a Git repository will create a `.git` directory but will not touch any of the already existing files.


## User Identification

This can be done either globally or for this repository only.
I prefer to do it per repository because I use different email addresses in different contexts.
```shell
/UnrealProjects/MyProject
$ git config --local user.name "Martin Nilsson"
$ git config --local user.email "ibbles@hotmail.com"
```


## Binary Files And Git LFS

Next we need to tell Git which files are binary files and should be tracked with Git LFS.
This is done with globbing patterns and is configured per Git repository.
```shell
/UnrealProjects/MyProject
$ git lfs track "*.uasset"
$ git lfs track "*.umap"
```

The quotes are important because we don't want the shell to expand this into an explicit list of files.
This produces a file named `.gitattributes` that should be tracked by Git.
```shell
/UnrealProjects/MyProject
$ cat .gitattributes
*.umap filter=lfs diff=lfs merge=lfs -text
*.uasset filter=lfs diff=lfs merge=lfs -text
$ git commit -m"Add .gitattributes for LFS tracking"
```

Anyone who wish to clone the repository must have Git LFS installed.


## Ignore Temporary Files

Unreal Engine creates a bunch of temporary working files that is not needed on other machines.
These files are placed in a specific set of directories that we want Git to ignore.
This is done by adding them to the `.gitignore` file.
I usually do this by opening `.gitignore` in a text editor.
We can also add files created by our IDE or other applications.
```shell
/UnrealProjects/MyProject
$ cat .gitignore
.idea
.vscode
*.code-workspace
Binaries
Content/Developers
DerivedDataCache
Intermediate
Makefile
Saved
$ git add .gitignore
$ git commit -m"Add .gitignore"
```


## Initial Commit

```shell
/UnrealProjects/MyProject
$ git add Config/ Content/ Source/ *.uproject
$ git commit -m"Initial commit"
```


## Publish

If the Git repository is to be published on e.g. GitHub then add the target URL as a remote.
```shell
/UnrealProjects/MyProject
$ git remote add origin URL
```
For example
```shell
/UnlrealProjects/MyProject
$ git remote add origin ssh://git@github.com/my_username/my_project
```

Push the repository to the remote with `git push`.
```shell
/UnrealEngine/MyProject
$ git push
```
