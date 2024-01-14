Freeze rendering is used to visualize what is currently being fed through the rendering pipeline.
By freezing rendering we can move the camera around and see which parts of our level has been culled away due to [[Frustum Culling]].
To freeze rendering run the `FreezeRendering` console command.
`Renderin frozen` will be printed in yellow in the top-left corner of the screen.

Things outside the view frustum may still be rendered because:
- They are batched with instances that are inside the view frustum.
- Any other reasons?

I'm not sure if this also freezes selected [[Mesh LOD]] levels.


# References

- [_Begin Play | Rendering_ 2:25 by Epic Games @ dev.epicgames.com 2022](https://dev.epicgames.com/community/learning/tutorials/vyZ1/unreal-engine-begin-play-rendering)
