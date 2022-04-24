Also called Trigger Volumes.
Box, Capsule, and Sphere shapes.
The [[Collision Settings]] can be set to **block, generate event, or ignore** based on what type of object intersects the collision volume.
These can be set either with a **Collision Preset** or individually per object type.
For events to be generated the **Generate Overlap Events checkbox** must be checked.
**Physics collisions** can still happen even with the Generate Overlap Events checkbox unchecked.

The **size** of the Collision Component is set in Details panel > Shape or with the Set Box Extent node for a Box Collision.

Various events based on the Collision Component can be added to a [[Blueprint Class|Blueprint Class's]] [[Event Graph]] by clicking the `+` under Events in the Collision Component's Details panel.
Common events are Begin Overlap and End Overlap.

We can also get a **list of all Actors currently overlapping** the Collision Component with the Get Overlapping Actors member function.
The Actor owning the Collision Component will always be inside that same Component and will therefor be included in the list.
