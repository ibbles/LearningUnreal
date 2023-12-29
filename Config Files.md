The config files store configuration settings.
They have the `.ini` suffix.
They are stored in `Config` directories.
There is a hierarchy of `Config` directories, with files in directories lower in this list override higher ones.
- `Engine/Config/Base*.ini`
- `Engine/PLATFORM/*.ini`
- `Project/Config/Default*.ini`
- `Project/PLATFORM/*ini`
- `Project/Saved/Config/PLATFORM/*.ini`

In the list above `*` is one of a collection of filenames storing different types of settings.
For example:
- `Editor`
- `Engine`
- `Game`
- `Input`
- `Scalability`

