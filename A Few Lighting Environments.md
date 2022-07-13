# Overcast day
Create and [[HDRI Backdrop]].
The HDRI Backdrop by itself provides a lot of light, but much if it is indirect which means that [[Lumen]] produces a fair bit of crawling.
Create a [[Rect Light]], not a [[Directional Light]], to add sun light.
A [[Rect Light]] is more like sun light being scattered through cloud cover.
A [[Directional Light]] is more like the sun on a clear sky.
Make the [[Rect Light]] large, and reduce the intensity a bit.
The [[Rect Light]] gives us more defined specular highlights on objects in the scene.
Enable [[Rect Light]] > Details panel > Light > Advanced > Cast Ray Traced Shadows.
You may need to add one [[Rect Light]] for every opening where light can enter your environment.

[_Lighting in Unreal Engine 5 for Beginners_ - Overcast Day, by William Faucher @ youtube.com](https://youtu.be/fSbBsXbjxPo?t=2362)


