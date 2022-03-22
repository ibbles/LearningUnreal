There are two assets related to sound:
- Wave
- Cue

A Sound Wave is the sound data, the audio samples.
A Sound Cue is a player for a Sound Wave.
A Sound Cue can alter the Sound Wave in various ways.

A sound can be played from a Blueprint using the Play Sound At Location function.
Play Sound At Location can play both Waves and Cues.
The sound exists separate from the Object that created it, so it will keep playing even if the Object is destroyed.
