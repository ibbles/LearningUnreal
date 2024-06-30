A plugin developed by Epic Games to simulate fluids using the [[Niagara]] particle system.
Both liquids and gasses.
Both 2D and 3D.
Based on grids.
Not sure if grid-only, or if there are neighbor based particle methods as well.
Beta in Unreal Engine 5.0, must be enabled from the [[Plugin|Plugins]] menu.
Since it is an engine plugin, to be able to see the [[Asset]]s included you need to enable [[Content Browser]] > top-right corner > Settings > (Show Engine Content)|(Shove Plugin Content).

Everything implemented using the Niagara building blocks.
Built upon [[Niagara Simulation Stage]].
Built upon [[Niagara Grid Collection]] data interface.
Provides reusable modules that can be used to build other effects.
Modules for
- Create grids.
- Initialize grid content.
- Modify grid content.
	- Dissipate
	- Advect
- Copy to Volumetric Render Targets.
	- Volumetric Render Targets can be used by a [[Material]].

Designed for tweaking and experimentation.
Provides data structures for 2D and 3D grids.
Intended as a learning resource.


# Creating A Fluid

A fluid simulation is housed within a [[Niagara System]].
Create a new [[Niagara System]] with [[Content Browser]] > right-click > _Create Basic Asset_ > Niagara System.
A [[Niagara System]] can be created from a set of [[Niagara Emitter]]s,
by selecting New System From Selected Emitter(s) in the [[Niagara System]] creation dialog.
Disable Library Only to see more emitters.
(
I don't know what Library Only does.
What Emitters are part of the library?
How do I make an Emitter be part of the library?
)
For example, to create a 3D gas simulation, select and add Grid 3D Gas Master Emitter.

Fluid simulation emitters are different from regular [[Niagara Emitter]]s in that they have the Emitter node > Emitter Summary > Selection panel > Simulation category.
(
I think different types of fluid emitters have different properties listed here.
For example, see _3D Gas Master Emitter_ in this note.
)

# 3D Fluids

This analysis is based on the _Grid 3D Gas Simple Particle Source_ Niagara System template.
I don't know which of these statements are for that particular template only, any 3D fluid template, any fluid template, or any Niagara System.

The 3D simulation takes place within a regular grid of cells, a.k.a. a bunch of stacked boxes.
These cells exists within a bounding box.
Each cell contains one sample point of the fluid.
The finer the grid, i.e. the smaller the cells, the more detailed effects can be captured in the simulation.
_Debug Visualization_ below describe how to visualize the size of the bounding box and the cells.
But it also makes it use more memory and computation time, i.e. control how expensive it is.
The resolution, i.e. the number of cells along the longest axis of the bounding box, is set at Details panel > Override Parameters > Resolution Max Axis.
Resolution Max Axis  should be kept fairly low, 512 is really high, 128 is low but usable.
The size of the bounding box, i.e. the region of space where the fluid exists, is set with Details panel > Override Parameters > World Space Size.
There is also a Source Offset but changing it doesn't move the fluid source, not sure what source it is changing.


## Data Setup

A 3D fluid simulation consists of two emitters: a source emitter and a grid emitter.
The latter may also be called a simulation emitter.
Data is created in the source emitter and then pushed to the grid emitter.

### Source Emitter

The source emitter and it specifies where fluid is spawned.
It drives data into the simulation.
It comes with a renderer that is typically disabled since we want to render the grid fluid, not the source particles.
Disable the grid emitter and enable the renderer in the source emitter to see these particles.
Helps with debugging.
The source emitter is responsible for setting a specific set of attributes that are read by the grid emitter.
These attributes are set by the _Set Fluid Source Attributes_ convenience [[Niagara Module]].
- Density
	- By setting this to 0.0 we get mostly fire.
- Temperature
	- Setting this to 0.0 causes the fluid to turn dark, i.e. it becomes smoke instead of fire.
	- Not entirely sure on the difference between Density and Temperature, based on this description.
	- In testing, setting Temperature to 0.0 really do give just smoke.
	- Changing Density didn't seem to do much.
- Velocity
- And more. When making our own grid emitters we can add  additional data to this list.

The _Set Fluid Source Attributes_ Module will ensure that the required Attributes exists.

While Niagara support both CPU and GPU particles, as of Unreal Engine 5.0 the fluid simulations only support GPU particles.
This means that Emitter > Sim Target must be set to GPU Compute Sim on all source emitters.


### Grid Emitter

The grid emitter specifies how already spawned fluid should move through the grid.
It reads data about the fluid from the source emitter.
A source emitter must be selected at Grid Emitter > Emitter Summary > Particle Source > Particle Read.
Type the name of the source emitter.
As of Unreal Engine 5.0 it seems we can only have one source emitter.


#### Simulation Parameters

The Simulation tab of the Selection panel holds a bunch of parameters that control the simulation.

- Vorticity Confinement: Small-scale detail in the fluid. Set small to get a smooth fluid and high to get a chaotic one. Too much and the simulation explodes. Highly dependent on the grid resolution, so decide on that before tweaking here.
- Pressure Solve Iterations: A quality vs performance setting. May need to increase this if you have collisions.
- Density Dissipation: Control how long the smoke hang around before dissipating.
- Temperature Dissipation: Control how long it takes for fire to turn to smoke.
- Velocity Dissipation: A drag force.
- Temperature Buoyancy: How much lighter hot particles are compared to cold particles, i.e. how fast the fluid rises.
- Density Buoyancy: Causes the fluid to sink. The opposite of Temperature Buoyancy, kinda.


#### Rendering Parameters



# 3D Gas Master Emitter

The 3D Gas Master Emitter defines a grid-based gas simulation within a bounding box.
Selection panel > Simulation has the following parameters:
- World Size: The size of the simulated volume in cm.
- Local Pivot: Where within the simulated volume the local origin should be.
	- In the range -0.5..0.5 for each axis. 
	- Set to (0.0, 0.0, -0.5) to make the simulation expand sideways and up from the [[Niagara System]]'s position.
- Resolution Max Axis: The number of cells along the largest axis.
	- The other axis we be assigned whatever number of cells fit.
	- The cells are cubes, i.e. have equal side lengths.
- Pressure Solve Iterations: The number of times the solver is run per tick.
- Dissipation Rate: How quickly values in the grid decay.
	- (What do we mean by "value"? What does a grid contain? What does it mean to decay?)
- Compensate For Actor Motion: Shift values in the grid between cells when the [[Niagara System]] is moved in the world so that cell data stay roughly stationary in world space.
	- Not sure what happens with cells that are shifted in. Set to zero?
	- This will kill fluid in regions of space that is first shifted out of the grid and then shifted back in. That data is lost.


# Rendering

Fluid self-shadowing is not enabled by default.
This makes lighting on the fluid flat and featureless.
This is a [[Light Source]] property.
[[Light Source]] > [[Details Panel]] > Light > Cast Volumetric Shadow.

A [[Lighting]] trick to render smoke from fire is to place a [[Light Source]] some distance under the fire and turn off Cast Static Shadows and Cast Dynamic Shadows.
This will make it appear as the light is coming from the fire.
Set [[Details Panel]]  > Light > Light Color to orange.

Use a [[Lighting Channel]] to control which lights can affect the fluid, and only the fluid.
A [[Light Source]] is assigned to [[Lighting Channel]]s at [[Details Panel]] > Light > Lighting channels.
A [[Niagara System]] is assigned to [[Lighting Channel]]s at [[Details Panel]] > Lighting > Lighting channels.
A [[Light Source]] will only illuminate an object, such as a fluid, if they share at least one [[Lighting Channel]].


# Debug Visualization

3D fluid systems are contained withing a simulation domain, a volume in which the fluid exists.
This volume is visualized as a red box, the simulation's bounding box.
The simulation domain is a regular grid of cells, a.k.a. a bunch of stacked boxes.
The size of each cell is shown on each face of the red bounding box.

# References

- [_Creating Fluid Simulation in UE5 | Inside Unreal_ by Epic Games, 2022 @ youtube.com](https://www.youtube.com/watch?v=k7WLE2kM4po)
- [_Niagara Fluids_ by Daniel Pearson, DevonPenney, Patrick.Kelly @ dev.epicgames.com/community 2022 UE5.2](https://dev.epicgames.com/community/learning/paths/mZ/unreal-engine-niagara-fluids)
