There (at least) are two [[Asset]] types related to sound:
- [[Sound Wave]]: Audio samples.
- [[Sound Cue]]: Sound Wave player.

See also [[MetaSounds]].

A [[Sound Wave]] is the sound data, the audio samples.
Sound Waves are typically imported from `.wav`, `.mp3`, or similar.
A [[Sound Cue]] is a player for a Sound Wave.
A Sound Cue is an Unreal Engine thing, not something that is imported.
Create with [[Content Browser]] > right-click > Sounds > Sound Cue.
A Sound Cue can alter the Sound Wave in various ways.

A sound can be played from a Blueprint using the Play Sound At Location node.
Play Sound At Location can play both Waves and Cues.
The sound exists separate from the Object that created it, so it will keep playing even if the Object is destroyed.

# References

- [_C++ For Unreal Engine (Part 2) | Learn C++ For Unreal Engine | C++ Tutorial For Unreal Engine_ by Nerd's Lesson, A.T. Chamillard @ youtube.com, 2022, UE4.27](https://youtu.be/IYJwU-rB2jA?t=14178)
