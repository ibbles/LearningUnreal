Cascaded Shadow Maps is a [[LOD]] system for [[Shadow Maps]].
As the camera moves farther and farther away from an object that is illuminated by a [[Light Source]] that uses cascaded shadow maps the shadow quality is reduced in stages.
(
I think it is the distance from the object.
It could also be the distance from the shadow.
)
(
Is [[Directional Light]] that only type of [[Light Source]] that can use cascaded shadow maps?
)
A lower-resolution [[Shadow Maps|Shadow Map]] is used for far-away objects.
A higher-resolution Shadow Map is used for objects close to the camera.
At the very last stage the shadow is removed completely.
By default there are three levels of shadow map resolution for a [[Directional Light]].

Cascaded Shadow Maps is only used if [[Project Settings]] > Engine > Rendering > Shadows > Shadow Map Method is set to Cascaded Shadow Maps.
The alternative is [[Virtual Shadow Maps]].
I'm not entirely sure how [[Distance Field Shadows]] and [[Ray Traced Shadows]] interact with this setting.

Cascaded shadow maps is a type of [[Dynamic Shadows]].
This means that the shadows will respond to animation in both [[Skeletal Mesh]] and [[Static Mesh]] with a [[Material]] that uses [[World Position Offset]].
A [[Directional Light]] with [[Light Mobility]] set to movable will by default use cascaded shadow maps.
(
Is this true also when using [[Lumen]]?
Is this true also when using [[Virtual Shadow Maps]]?
)


# Configuration

The cascaded shadow maps for a [[Light Source]] can be configured in Light > Details panel > Cascaded Shadow Maps.

## Dynamic Shadow Distance

The maximum distance from the camera (not the light source) that shadows will be rendered.
If the camera is farther away then this from the object then there will be no shadow.
This also controls the transition points between the shadow map resolutions.
If we set a small Dynamic Shadow Distance then all shadow map transitions will be crammed into that short range.
If we set a large Dynamic Shadow Distance then each [[LOD]] level is used for a longer distance.

Dynamic Shadow Distance also influences the shadow sharpness for a given distance to the camera.
If the Dynamic Shadow Distance is reduced then the shadows become sharper.
I think this is an additional effect and not a side-effect of the changing transition points between shadow map resolutions.
But I don't understand why this effect happens.
Something about precision.

So Dynamic Shadow Distance is a balance between how sharp shadows you want when close to the object,
versus how early it is OK to completely remove the shadow when the camera pulls away from the object.

The Dynamic Shadow Distance affects performance.
If you set it low then many objects won't have a shadow at all and performance is high.
If you set it high then many objects will have a shadow and performance is low.

So:
- Low: High quality shadows, good performance, but only a few close-by objects will have any shadow.
- High: Low-quality shadows, poor performance, shadows on many objects even far away.

If the [[Light Source]] has [[Distance Field Shadows]] enabled then that shadow type will take over once past Dynamic Shadow Distance for the Cascaded Shadow Maps.
If the [[Light Source]] has [[Distance Field Shadows]] disabled then the object will be shadow-less once past Dynamic Shadow Distance for the Cascaded Shadow Maps.

## Num Dynamic Shadow Cascades

The number of shadow maps to create for this light.


# References

 - [_Lighting Essential Concepts and Effects - Dynamic Lighting - Outdoor_, by Epic Games @ dev.epicgames.com, 2018](https://dev.epicgames.com/community/learning/courses/Xwp/lighting-essential-concepts-and-effects/V0M/dynamic-lighting-outdoor)
