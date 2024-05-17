A light source is something that produces [[Light]], which adds [[Lighting]] to the scene.

A light source can either be a light [[Actor]] or [[Component]], or an object with an [[Emissive Material]]
Examples of light Actor or Components:

- [[Point Light]]: A light source that emanate light in equal amount in all directions, like a sphere.
- [[Directional Light]]: A light source that appears to be very far away, producing parallel light rays covering the entire extent of the scene.
- [[Spotlight]]: Like a Point Light but with a limited arc where it emanates light.
- [[Sky Light]]: A light source simulating light coming from a sky dome. Can either use a cube map or do real time scene capture.
- [[Rect Light]]: 

Light sources can be created from the Place Actors panel, the Add Component menu on an [[Actor]] or from the [[Environment Light Mixer]].

A Light Source has a bunch of [[Light Source Properties]].

Lights can have a big impact on performance.
See
- [[Lighting And Shadow Performance]]
- [[Rendering Performance Profiling And Analysis]]

A scene often consists of multiple light sources.
Common lighting setups can be created from the [[Environment Light Mixer]].

# Environment Light Mixer

See [[Environment Light Mixer]].
Opened from Top Menu Bar > Window > Env. Light Mixer.
The Environment Light Mixer contains buttons for creating a number of common light sources.

