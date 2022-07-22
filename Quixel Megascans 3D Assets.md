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
