I use auto exposure to refer to all types of adaptive brightness control.

We can visualize the current ??? with [[Level Viewport]] > [[Show]] > Visualize > HDR (Eye Adaptation).

When designing a level it can be distracting to have the brightness go up and down all the time.
There are various ways to disable this.


# View Mode

[[View Mode]] > Exposure > Fixed at some value.

# Show

[[Show]] > Exposure > untick Game Settings.

[[Show]] is the button in the upper-left corner of the Level Viewport next to [[View Mode]].

This is only for the editor, has no effect on exported applications.

# Post Process Volume

One type of auto exposure is controlled from a [[Post Process Volume]].
There are two ways to disable this auto exposure feature, both from the Post Process Volume's Details panel, the Exposure category.

## Manual Metering Mode

	- Set Metering Mode to Manual.
	- Set Apply Physical Camera Expose to disabled.
	- Adjust Exposure Compensation to your preference.


## Zero Range Min Max

- Leave Metering Mode at Auto Exposure Histogram.
- Set Min Brightness and Max Brightness to the same value.
- Adjust Min Brightness and Max Brightness together to your preference.


# Cine Camera Actor

If you use a Cine Camera Actor then the exposure can be controlled from its Details panel.
Set Post Process > Lens > Exposure > Min|Max EV100 to the same value.
I don't know how this interacts with the [[Post Process Volume]]'s exposure settings.
I don't know the unit of EV100.
It allows negative values.
Smaller values makes the scene brighter.
-5 to 20 seems to be plausible values.


# References

- [_Let's Create an Unreal Engine 5 Environment in ONE SITTING Ep.2 | 3D Livestream_ - Auto Exposure, by pwnisher @ youtube.com](https://youtu.be/k77o5Ug41ek?t=1304)
- [_Lighting in Unreal Engine 5 for Beginners_ - Disable Auto Exposure, by William Faucher @ youtube.com](https://youtu.be/fSbBsXbjxPo?t=1846)
- [_Introducing Global Illumination_ - Visualizing Lightmass, by Epic Games @ dev.epicgames.com, UE-4.20](https://dev.epicgames.com/community/learning/courses/yon/introducing-global-illumination/x7n/visualizing-lightmass)
- [_Introducing Post Processing_ by Epic  Online Learning, Kevin Lyle @ dev.epicgames.com/courses 2023 UE5.0](https://dev.epicgames.com/community/learning/courses/pE2/unreal-engine-introducing-post-processing/mZ11/unreal-engine-introducing-post-processing-overview)

