Movie rendering is the act of rendering out a sequence of frames to a regular video file.
In Unreal Editor we do this with the Movie Render Queue plugin and a [[Level Sequence]].
The [[Level Sequence]] determine how many frames should be exported and what should happen in the world during those frames.
Interpolations and such.
A [[Level Sequence]] is an [[Asset]] that is created in the [[Content Browser]].
They are created with [[Content Browser]] > right-click > Animation > Level Sequence.
Opening a [[Level Sequence]] asset opens the [[Sequencer]] panel.

Set a frame rate in the Sequencer panel tool bar.
Create a [[Cine Camera]] Actor in your level and drag it into the left part of the Sequencer panel.
The camera's view shows up in the right part of the Sequencer panel, in the timeline.


# Key Frames

Some of the [[Property|Properties]] of the Camera are listed in the left part of the Sequencer panel and as tracks in the right timeline part.
We can set key frames for these properties in the timeline.
- Move the track head at the top of the timeline to the time where you want the key frame.
- Set the value you want for the property at that key frame.
- Click the ⊕ button on the property's row. A red/pink circle will appear on the time line.


# Movie Render Queue

Click the `⋮` next to Sequencer panel > tool bar > clap board button and select Movie Render Queue.
Clicking the clap board button after will open the Movie Render Queue panel.
The Movie Render Queue has a bunch of settings.
These can be saved and loaded as presets.
The preset can include [[Console Variable|Console Variables]].

## Settings

### Anti-Aliasing

There are two anti aliasing settings.
Spatial Sample Count renders each pixel multiple times slightly shifted on the screen and averages the result.
Temporal Sample Count renders each pixel multiple times slightly shifted in time and averages the result.
So if you set Spatial Sample Count to `m` and Temporal Sample Count to `n` then each pixel is rendered `m*n` times.

A high Spatial Sample Count gives crisp detailed images without aliasing, flickering, and other noise.
A high Temporal Sample Count gives better motion blur.
(
Is this really true?
)

The Override Anti Aliasing setting means that the Anti Aliasing Method setting takes precedence over the [[Project Settings]] setting. (I guess.)
Not sure how the Anti Aliasing Method setting interacts with the Spatial Sample Count and Temporal Sample Count settings.
If they are independent, or if the count settings are passed to the chosen anti aliasing method.

In addition, one can add the `r.screenPercentage <NUMBER>` [[Console Variable]] to Move Render Queue > Settings > Settings (green button) > Console Variables.
I think that is in addition to Spatial Sample Count, not sure though.

[_Troubleshooting FOLIAGE issues in Unreal Engine_ - Screen Percentage, by William Faucher @ youtube.com](https://youtu.be/Ar3vvygirLU?t=759)

### Game Mode Overrides



## Out Of VRAM

If you run out of VRAM at very start of the render then untick the following in Game Overrides:
- Flush Grass Streaming
- Flush Streaming Managers

# References

- [_Let's Create an Unreal Engine 5 Environment in ONE SITTING Ep.2 | 3D Livestream_ - Movie Render Queue, by pwnisher @ youtube.com](https://youtu.be/k77o5Ug41ek?t=9336)

