The Stat system collects a bunch of statistics about the operation of the engine and surrounding systems [(1)](https://dev.epicgames.com/documentation/unreal-engine/stat-commands-in-unreal-engine).
These are grouped into categories.
We can display the collected data with the `stat <CATEGORY>` console command.
The data shows up as a table rendered onto the viewport.

- `stat none`: Clear the screen of all statistics.
- `stat fps`: Show an FPS and frametime counter to the right of the [[Level Viewport]].
- `stat scenerendering`: A summary of the scene rendering, both timing and contents.
- `stat game`: Blueprint time and such.
- `stat gpu`: 
- `stat unit`: Milliseconds per thread. Useful for knowing where to start looking for bottlenecks.
- `stat memory`: Shows memory usage, e.g. how much is used by textures.
- `stat streaming`: Statistics about the texture streaming system.
> TODO List a few more here.

# References

- 1: [_Stat Commands_ by Epic Games @ dev.epicgames.com/documentation 2026](https://dev.epicgames.com/documentation/unreal-engine/stat-commands-in-unreal-engine)
