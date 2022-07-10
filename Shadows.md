Shadows can be either [[Dynamic Shadows|Dynamic]] or [[Static Shadows|Static]].


# Soft Shadows

For many [[Light Sources]] the shadows are very hard-edged by default.
They can be made soft by increasing the Light's Details panel > Light > Source Radius.
For this to work the light must be using a shadow method that supports soft shadows.
The following settings are relevant:
- Light > Details panel > Distance Field Shadows
- Project Settings > Engine > Rendering > Shadows > Shadow Map Method

You get soft shadows with
- Alternative 1
	- Distance Field Shadow > Enabled
	- Shadow Map Method > Irrelevant.
	- The per-light Distance Field Shadow setting override the project-wide Shadow Map Method.
- Alternative 2
	- Light > Details panel > Distance Field Shadows > Disabled
	- Shadow Map Method > Virtual Shadow Maps
- 

# References

- [_ Lighting in Unreal Engine 5 for Beginners _ by William Faucher @ youtube.com](https://youtu.be/fSbBsXbjxPo?t=606)

