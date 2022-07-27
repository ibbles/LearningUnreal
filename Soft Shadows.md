For many [[Light Source|Light Sources]] the shadows are very hard-edged by default.
They can be made soft by increasing the Light's Details panel > Light > Source Radius.
On a [[Directional Light]] this is called Source Angle instead.
For this to work the light must be using a shadow method that supports soft shadows.
The following settings are relevant:
- Light > Details panel > Distance Field Shadows.
- Project Settings > Engine > Rendering > Shadows > Shadow Map Method
- (I think ray traced shadows can also be soft, my I haven't been able to figure out how to enable that yet.)

You get soft shadows with
- Alternative 1
	- Distance Field Shadow > Enabled
	- Shadow Map Method > Irrelevant.
	- The per-light Distance Field Shadow setting override the project-wide Shadow Map Method.
- Alternative 2
	- Light > Details panel > Distance Field Shadows > Disabled
	- Shadow Map Method > Virtual Shadow Maps
- (Alternative 2)
	- (Enable ray traced shadows.)
	- (I assume, haven't gotten that to work yet.)

Note that a [[Directional Light]] that has Details > Atmosphere And Cloud > Atmosphere Sun Light enabled will increase in size on the sky when [[Directional Light]] > Light > Source Angle is increased.

# References

- [_Lighting in Unreal Engine 5 for Beginners_ - Shadows, by William Faucher @ youtube.com](https://youtu.be/fSbBsXbjxPo?t=606)
