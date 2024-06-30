This note describe how to achieve various effects using a [[Niagara System]] and [[Niagara Emitter]]s by implementing and combining [[Niagara Module]]s.

We can control the motion of our particles in three main ways:
- Position
	- Set a new position directly.
- Velocity
	- Set a velocity on the particle and integrate to compute the new position.
- Force
	- Set a force on the particle and integrate to compute the new velocity, and then integrate again to get the new position.

The integration is handled by a built-in [[Niagara Module]] named Solve Forces And Velocity.

Velocity and / or force can either be set in the Particle Spawn section to give the particles an initial impulse,
or in Particle Update to affect them continuously.


# Add Velocity

Add Velocity is a built-in [[Niagara Module]] that adds to the PARTICLES > Velocity [[Niagara Attribute]].
It has a float parameter named Velocity Speed that is the speed, not velocity, that is added to the particles.
The direction can be set in various ways, and is controlled with Velocity Mode.
- From Point
	- The added velocities are pointing away from Velocity Origin. This is set to ENGINE > EMITTER > Simulation Position by default. I think this is the position of the System in the world, but its a weird name so I'm not sure.
	- There is a Origin Offset that lets  you offset the point from the Velocity Origin value.
- TODO Fill in here Velocity Modes here.

This [[Niagara Module]] is often used with the Particle Spawn stage to give new particles an initial velocity.
By setting Velocity Mode to From Point and setting Origin Offset to some small-ish value (tens) we get a directional burst in the opposite direction.
Set the Origin Offset down and we get an initial velocity upwards.
Set the Origin Offset left and we get an initial velocity to the right.


# Collisions

A [[Niagara Module]] that adds collisions.
Should be added to the Particles Update stage.
I assume before Solve Forces And Velocity.
The types of collisions that can be generated depend on if the [[Niagara Emitter]] uses CPU Sim or GPUCompute Sim.
- CPU
	- Ray Traced: Does a [[Line Trace]] using the World Dynamic trace channel.
		- This sounds really expensive.
	- Analytical Planes: Add a bunch of infinite planes that approximate the world.

Collision Radius > Radius Calculation Type should be set to Mesh to use the Static Mesh selected in the [[Niagara Mesh Renderer]] to compute the size of the particles.
At least that's what I though it did, but the [_Intro To Niagara_ ](https://dev.epicgames.com/community/learning/tutorials/8B1P/unreal-engine-intro-to-niagara) video still set the Mesh Dimensions explicitly, by hand, in the Collision Module's Selection panel.



# Curl Noise Force

Uses a 3D generated noise to apply a different force to each particle,
producing a swirling pattern.

- Noise Strength: How much force to apply.
- Noise Frequency: With lower frequency the particles move in bigger groups. With larger frequency there is a higher difference in force for neighboring particles.

Supports debug drawing.
Enable debug drawing with the eye button on the [[Niagara Module]] in the [[Niagara Emitter]]'s module stack.
Will draw lines in the preview [[Viewport]] showing the direction of the applied forces,
colored based on the force magnitude.

# Drag

Add a force to the particles.
The force is dependent of a Drag float parameter and the velocity of each particle.
The drag force is always directed opposite to the velocity, thus acting to slow the particle down.
By having a high Drag parameter we can have particles that start of with a high velocity but quickly slows down.


# Gravity Force

Add a force to the particles.
The force is dependent on a Gravity vector parameter and the mass of each particle.
Set the Gravity to some negative value in the Z direction to get falling particles.
Gravity Force is usually added to the Particle Update stage since gravity is always pulling.
Relentlessly, tirelessly, eternal.

# Initialize Particle

A very versatile [[Niagara Module]] that can set a whole  bunch of [[Niagara Attribute]]s in various ways.


# References

- [_Intro To Niagara_ by Epic Online Learning, James Hill @ dev.epicgames.com/tutorials 2023 UE5.2](https://dev.epicgames.com/community/learning/tutorials/8B1P/unreal-engine-intro-to-niagara)
