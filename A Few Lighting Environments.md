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

If you do use a [[Directional Light]] for your sun light, you can make it more overcast-y by adding a [[Sky Atmosphere]].
On the [[Sky Atmosphere]] increase Details panel > Atmosphere - Mie > Mie Scattering Scale.
This will diffuse the light, making shadows stand out less.
They will still be sharp-edged, but the non-shadow parts will be muted.
(
What's the difference from just reducing the intensity of the light, or the [[Post Process Volume]] > Exposure Compensation?
)
See also [[Soft Shadows]].

- [_Lighting in Unreal Engine 5 for Beginners_ - Overcast Day, by William Faucher @ youtube.com](https://youtu.be/fSbBsXbjxPo?t=2362)
- [_Let's Create an Unreal Engine 5 Environment in ONE SITTING | 3D Livestream_ - Overcast lighting, by pwnisher @ youtube.com](https://youtu.be/02tEDwb8960?list=PLkDceauvDXDy23KPR7tU2lkA9C69MzI0k&t=1139)



