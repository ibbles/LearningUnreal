The part of the Quixel Megascans library that contains [[Static Mesh Asset]]s.



# Blurry Textures

Sometimes these 3D assets become blurry in the [[Viewport]].
It looks like the textures aren't being loaded properly.
One possible cause is that a [[Texture]] that should not be [[Virtual Texture]] accidentally has become marked as such.
You can verify if this is the case by the presence of the VT decorator on the texture's icon in the [[Content Browser]].
To find the texture, open the [[Material]] used for the Quixel Megascans [[Static Mesh]].
In the Textures section of the parameter list, click the Browse To Asset button next to any texture, the folder opened in the Content Browser usually hold all textures.
If any of the textures has the VT decorator, then open it in the [[Texture Editor]] and disable Details panel > Texture > Virtual Texture Streaming.

You can use the [[Bulk Property Editor]] to edit many textures at once.

Another option, instead of turning Virtual Textures into regular textures, is to change the [[Material]] to use Virtual Texture sampling.
This is described in the [_Unreal Engine 5: updated FIX for blurry highres megascans textures (RVT)_](https://www.youtube.com/watch?v=QGn9d1fFujI) video, but I don't understand what is happening.
A bunch of textures are duplicated and then the copy is made into a [[Virtual Texture]].
But wasn't the problem that the texture got marked as a [[Virtual Texture]] already?
Why do we need a duplicate if the original texture is already a [[Virtual Texture]]?
And if it isn't a [[Virtual Texture]], why do we have blurriness?
Or is all the copying more of a safety thing, by manually making all textures Virtual Textures we ensure that they stay that way, and that all texture are configure the same?

- [_Unreal Engine 5: Fix for blurry highres megascans textures (RVT) !CHECK PINNED COMMENT FOR UPDATE!_ by hcaq @ youtube.com 2022](https://www.youtube.com/watch?v=e19e6XKk6Wc)
- [_Unreal Engine 5: updated FIX for blurry highres megascans textures (RVT)_ by hcaq @ youtube.com 2022](https://www.youtube.com/watch?v=QGn9d1fFujI)


