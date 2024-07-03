A plugin developed by Epic Games to simulate fluids using the [[Niagara]] particle system.
Must be enabled from the [[Plugin|Plugins]] menu.
Both liquids and gasses.
Both 2D and 3D.
Based on grids.
Not sure if grid-only, or if there are neighbor based particle methods as well.
Since it is an engine plugin, to be able to see the [[Asset]]s included you need to enable [[Content Browser]] > top-right corner > Settings > (Show Engine Content)|(Shove Plugin Content).

Everything implemented using the Niagara building blocks.
Built upon [[Niagara Simulation Stage]].
Built upon [[Niagara Grid Collection]] data interface.
A simulation contains multiple grids, each holding one piece of data.
These are called Grid Attributes.
- Density
- Velocity
- Temperature
- Color

Provides reusable [[Niagara Module]]s that can be used to build other effects.
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

## From A Template

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


## From An Empty Emitter

### Emitter Asset

Create a [[Niagara Emitter]] with [[Content Browser]] > right-click > _Create Advanced Asset_ > FX > Niagara Emitter.
Set Emitter node > Properties > Selection panel > Emitter > Sim Target to GPU Compute Sim.
Required because simulation grids are only supported using GPU Compute Sim.

### Grid Setup

#### Grid Emitter Attribute

A grid is created with Parameters panel > Emitter Attributes > + button > Make New > Grid3D Collection.
Give it a name, for example grid.
The full name will be, for example, EMITTER | Grid.

To create the grid object, drag Parameters panel > Emitter Attributes > EMITTER | Grid into the Emitter Spawn stage of the Emitter's node.
This will create a Set: EMITTER | Grid [[Niagara Module]].
Set Selection panel > Set (EMITTER) Grid > EMITTER | Grid > Grid > Num Attributes to 0.
The attributes are created elsewhere.

#### Grid Resolution

To initialize the grid add a Grid 3D Set Resolution [[Niagara Module]] to the Emitter Spawn stage.
Place it after the Set: EMITTER | Grid Module.
To make the Grid 3D Set Resolution Module act on the grid created in the Set: EMITTER | Grid Module, select the Grid 3D Set Resolution Module and click Selection panel > Grid > ⌵ button > Link Inputs > Emitter > EMITTER | Grid.
The last stage of that path is the name you gave to the grid in Parameters panel > Emitter Attributes when it was first created.

How the resolution of the grid is set depends on what Grid 3D Set Resolution > Selection panel > Resolution Method is set to.
- Independent: Each axis has it's own and independent number of cells and world extent.
	- I assume this means that the cells can be non-cubic, i.e. oblong.
- Max Axis: Set the number of cells for the longest world extent axis.
	- The cells are made cubic and the other two axes get as many cells as will fit.
- World Cell Size:
- Other Grid: Make the grid resolution equal to that of some other grid.
- Volume Texture: Make the grid resolution equal to a Volume Texture.
Num Cells \[XYZ\] are the number of cells in the X, Y, and Z direction.
World Grid Extents is the size of the grid in world space.
Since we can set both the number of cells and grid extents independently for all directions, does that mean that the grid can have non-cube cells?

If is often useful to make Grid3D Set Resolution > World Grid Extent a User Parameter,
since we may need to use this value in other [[Niagara Module]]s as well.
Click the ⌵ button > General > Make > Read From New Emitter Parameter.
The new parameter will show up at Parameters panel > Emitter Attributes > EMITTER | World Grid Extents.
To set a default value to this parameter, drag it from the Parameters panel into the Emitter Spawn stage, immediately before the Grid 3D Set Resolution Module.

### Grid Initialization Stage

Click Emitter node > + Stage button > Simulation Stages > Generic Simulation Stage.
This adds a None state go the Emitter node that contains a Generic Simulate Stage Settings entry.
Select Generic Simulation Stage Settings to be able to set some settings in the Selection panel.
- Simulation Stage
	- Simulation Stage Name: The name of this stage.
		- Is displayed in the Emitter node.
	- Enabled Binding
	- Num Iterations
	- Num Iterations Binding
	- Iteration Source
		- Particles: Run the stage logic for every particle.
			- Selecting this enables the Particle Parameters category.
				- Particle Iteration State Enabled.
		- Data Interface: Run the stage logic for every element in a data container.
			- Selecting this enabled the Data Interface Parameters.
			- Data Interface: The data container to run the logic for.
				- This will find the grid that was created in Parameter panel > Emitter Attributes.
			- Execute Behavior:
				- Always:
				- On Simulation Reset: Select this for Simulation Stages that handles grid initialization.
				- Not On Simulation Reset: Select this for Simulation Stages that handles tick updates.

Create a new parameter on the grid with Emitter node > Grid Initialization Stage > + button > Set New Or Existing Parameter Directly.
Grid Initialization Stage > Set Parameters > + button > Make New > Vector.
By default the parameter is created in the PARTICLES [[Niagara Namespace]],
which means that the variable is created per-particle.
We don't have particles, we have a grid with cells.
To turn it into a grid (Or cell?) parameter right-click > Namespace > STACKCONTEXT.
STACKCONTEXT means "Assign to to whatever the current stack is related to, its context.".
For a simulation stage with Generic Simulation Stage Settings > Selection panel > Simulation Stage > Iteration Source set to Data Interface, the context is whatever data container Simulation Stage Settings > Selection panel > Data Interface Parameters > Data Interface is set to.
If this is a grid will a million cells, then a million parameter values are created.
The new parameter will show up at Parameters panel > Emitter Attributes > EMITTER | GRID | Parameter Name.
For modules in a Generic Simulation Stage with the Iteration Source set to a grid Data Interface, STACKCONTEXT and EMITTER | GRID are synonymous.
The parameter can be renamed either from the Parameters panel or from the Set [[Niagara Module]].


### Grid Tick Stage

Create a new simulation stage with Emitter node > + Stage button > Simulation Stages > Generic Simulation Stage.
Give it a name in Generic Simulation Stage Settings > Selection panel > Simulation Stage Name.
Set Simulation Stage > Iteration Source to Data Interface.
Set Data Interface Parameter > Data Interface to EMITTER | Grid.


# Emitter / Source

In Niagara Fluids an Emitter is also called a Source.
Where regular [[Niagara Emitter]] spawn particles, a fluid simulation Emitter instead writes data into the grid.
The Grid 3D Gas Master Emitter, and possibly others, sources are configured in Selection panel > Sphere Source and Selection panel > Particle Source.

## Sphere Source

There is a sphere source that writes sphere shaped volumes.
Has a position and a scale.
Can write density, temperature, and/or color.

## Particle Source

Uses a regular [[Niagara Emitter]] to write data into the grid.
Require that the [[Niagara Emitter]] has Emitter node > Properties > Selection panel > Emitter > Sim Target set to GPUCompute Sim.
Since the fluid simulation run on the GPU.
(
[_Niagara Fluids_ @ dev.epicgames.com/community](https://dev.epicgames.com/community/learning/paths/mZ/unreal-engine-niagara-fluids) says:

> Select the **Grid3D Gas Master Emitter** from the **Parent Emitters** tab.

but I need to experiment to understand what that means.
)

The Grid 3D Gas Master Emitter, and possibly others, have Selection panel > Particle Source > Read Particle Source which should be enabled to have a regular [[Niagara Emitter]] write into the Attribute Grids.
Enabling Selection panel > Particle Source > Read Particle Source causes a bunch of additional properties to appear in the Particle Source category, and a Particle Reader category to appear.
To make a [[Niagara Emitter]] write into the grid write the name of the [[Niagara Emitter]] into the grid node's Selection panel > Particle Reader > Particle Read Emitter Name.
This isn't an array, so not sure what to do if we need to read from multiple [[Niagara Emitter]].

The actual writing is done in the [[Niagara Emitter]], using a Set Fluid Source Attributes [[Niagara Module]] in the Particle Update stage.
Set Fluid Source Attributes has the following properties in the Selection panel:
- Density
- Temperature
- Velocity
- Velocity Scale
- Divergence
- Color
- Radius

These values can either be set to a static value or bound to a [[Niagara Dynamic Parameter]].
For example, it is common to bind to particle attributes.
Velocity can be bound to PARTICLES | Velocity and Color to PARTICLES | Color.
If you write temperature and/or color in the Set Fluid Source Attributes [[Niagara Module]] then you probably want to enable Grid Emitter > Emitter Summary > Selection panel > Attributes > Density and/or Temperature.


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
Emitter Summary > Selection panel > Simulation has the following parameters:
- World Size: The size of the simulated volume in cm.
- Local Pivot: Where within the simulated volume the local origin should be.
	- In the range -0.5..0.5 for each axis. 
	- Set to (0.0, 0.0, -0.5) to make the simulation expand sideways and up from the [[Niagara System]]'s position.
	- See _Grid Unit Space_ in this note.
- Resolution Max Axis: The number of cells along the largest axis.
	- The other axis we be assigned whatever number of cells fit.
	- The cells are cubes, i.e. have equal side lengths.
- Pressure Solve Iterations: The number of times the solver is run per tick.
- Dissipation Rate: How quickly values in the grid decay.
	- (What do we mean by "value"? What does a grid contain? What does it mean to decay?)
- Compensate For Actor Motion: Shift values in the grid between cells when the [[Niagara System]] is moved in the world so that cell data stay roughly stationary in world space.
	- Not sure what happens with cells that are shifted in. Set to zero?
	- This will kill fluid in regions of space that is first shifted out of the grid and then shifted back in. That data is lost.
	- Can be combined with Emitter Summary > Selection panel > Actor Motion > Calculate Actor Motion Force and Actor Motion > Multiplier.  This adds a force to the fluid so that it is dragged along with the [[Niagara System]] [[Actor]]. With Multiplier set to 1.0 the fluid follows kinematically with the [[Actor]]. When set to something smaller the fluid follows, but slightly slower / behind.

Emitter Summary > Selection panel > Attributes has the following parameters:
- Density: Allocate and update an Attribute Grid for density.
- Temperature: Allocate and update an Attribute Grid for temperature.
	- Enabling this, and writing a temperature close to 1.0, causes the simulation to look like fire. The temperature to write may depend on properties set on the renderer module.
- Interpolation Type: How to sample the grid when rendering.
	- Linear: Faster, but tends to smooth away details.
	- Cubic: Slow, but gives more structure / detail to the fluid.
- Dissipation Rate Density: How fast density disappears from the simulation.
	- If your grid becomes filled with smoke then increase this value towards 1.0.

Emitter Summary > Selection panel > Color has the following parameters:
- Color: Allocate and update an Attribute Grid for color.

## Grid 3D Gas Master Emitter

(
I don't know how much of this applies to other Grid Emitter types as well.
)

The Grid Emitter contains a bunch of rendering settings at Selection panel > (Render Color)|(Render Density)|(Render Temperature).


# Writing Attributes

## Sphere Source

This is set of properties at Grid Node > Emitter Summary > Selection panel > Sphere Source.

## Particle Source Reader

This is a collaboration between the Grid Emitter and a [[Niagara Emitter]].
The [[Niagara Emitter]] has in its Particle Update stage a Set Fluid Source Attributes [[Niagara Module]].
The Grid Emitter has Emitter Summary > Selection panel > Particle Source > Read Particle Source enabled and Particle Source > Particle Reader > Particle Read > Emitter Name set to the name (top of the node) of the [[Niagara Emitter]] that the Set Fluid Source Attributes [[Niagara Module]] was added to.

Enabling Emitter Summary > Particle Source > **Deterministic Source** causes a deterministic, but slower, write implementation to be used when writing data from the [[Niagara Emitter]] to the grid.
All possible results from the non-deterministic write implementation are valid so this setting can usually be turned off.

Enabling Emitter Summary > Particle Source > **Use Streaking** enables continuous sourcing.
This means that the entire trajectory a particle traveled between two ticks is written,
as opposed to only where the particle is at each tick.
Similar in concept to continuous collision detection.
Not sure if this looks at current and prior positions, or does reverse integration based on current velocity.
While this prevents gaps you may still get a non-smooth appearance since the each write is a sphere (Can it be other shapes?) and a row of spheres creates a wave effect.

Enabling Emitter Summary > Particle Source > **Use Radius Falloff** causes the "brush" that writes values to the grid at the particle positions to be smoothed, i.e. the full value is only written at the center of the particle and decreases towards its surface.

Enabling Emitter Summary > Particle Source > **Read Divergence** causes the particle to act as a density repellent.
The higher Divergence set in Set Fluid Source Attributes the more aggressive the particle will push the fluid away.
Set a negative value to make the particle act as an attractor instead.
In the demo in  [_Niagara Fluids_ @ dev.epicgames.com/community](https://dev.epicgames.com/community/learning/paths/mZ/unreal-engine-niagara-fluids) the particles with negative Divergence caused fluid to disappear.
Not sure what is causing that or how to prevent it.
Grid Emitter > Emitter Summary > Selection panel > Simulation > Dissipation Rate?

# Using Particle Modules

Some [[Niagara Module]]s designed to be used with particles can be used with a grid as well.
Here using the Curl Noise Force module as an example.
Such modules can be added to a simulation stage just like they can be added to e.g. the Particle Update stage.
You may get a warning in the Selection panel about unmet dependencies.
This warning is based on particle based simulations and can be dismissed for grid based simulations.

We need to tell the module not to read and write particle data and use grid data instead.
For a Curl Noise Force this is the position to sample and the resulting force.
To do this we need to show the advanced parameter by clicking the ⌵ button in the bottom-center.
Disable Curl Noise Force > Write To Intrinsic Parameters.
(
I don't know what an "intrinsic" parameter is.
Does it mean "the particle attributes"?
)
Change the Sample Position parameter.
Sample Position is by default set to Simulation Position.
We want it to sample the world space location of each grid cell.
Select Sample Position > ⌵ button > Dynamic Inputs > Grid 3D World Position.
This create a bunch of Dynamic Input Parameters.
- Grid: The grid to sample from.
	- ⌵ button > Link Inputs > Emitter > EMITTER | Grid, or whatever you named your Emitter Parameter for the grid to.
- Unit To World: Transformation matrix converting grid internal space coordinates to world space coordinates.
	- See _Grid Unit Space_ and then ⌵button > Link Inputs > Emitter > EMITTER | GRID 3D CREATE UNIT TO WORLD TRANSFORM > Unit To World.

The output from such modules are stored somewhere where they can be linked to from other module parameters.
In the Curl Noise Force example the output can be linked to with ⌵ button > Link Inputs > Output > OUTPUT | CURL NOISE FORCE | Curl Nose Force.


# Collision Detection

## Collisions With Particles

A fluid simulation can do collision detection against particles from a [[Niagara Emitter]] in the same [[Niagara System]].
The [[Niagara Emitter]] must have Properties > Selection panel > Emitter > Sim Target set to GPU Compute Sim.

Enable the Grid Emitter > Emitter Summary > Selection panel > Collide Against > Use Particle Collisions.
This will enable the Collision Data Interfaces > Collision Particle Reader group of properties.
Set Collision Data Interfaces > Collision Particle Reader > Particle Read > Emitter Name to the name of the [[Niagara Emitter]] to calculate collisions against.


## Collision With Static Meshes

A fluid simulation can do collision detection against [[Static Mesh]]es in the scene.
This is enabled at Grid Emitter > Emitter Summary > Collide Against > Use Mesh Collisions.
By default it will collide against the [[Mesh Distance Field]].
To use the mesh's [[Collision Shape]]s instead untick Use Mesh Distance Fields.
(
I assume this will use the collision primitives,
but I haven't actually tested.
)

Collision detection can become very expensive if there are many [[Static Mesh]]es within the fluid's simulation grid volume.
You can use Component Tags to control which [[Static Mesh]]es are tested for collisions.
- Decide on a tag to use, such as `Fluid Collider`.
- Add that tag to the [[Static Mesh]] > Details panel > Tags > Component Tags array.
- Add that tag to the Grid Emitter > Emitter Summary > Collision Data Interfaces > Collision Rigid Mesh Query > Source > Component Tags array.

When the Grid Emitter > Emitter Summary > Collision Data Interfaces > Collision Rigid Mesh Query > Source > Component Tags array is non-empty only [[Static Mesh Component]]s with any (or all?) of those tags on them will be considered.
The the Component Tags array is empty all [[Static Mesh Component]]s are considered.

Enabling Use Mesh Collisions will not only enable collisions with [[Static Mesh]]es but also the capsule collisions of [[Skeletal Mesh]]es.

## Collision With Skeletal Meshes

### Grid 3D Gas

A fluid simulation can do collision detection against the capsule colliders of a [[Skeletal Mesh]].
This is enabled at Grid Emitter > Emitter Summary > Collide Against > Use Mesh Collisions.
This will also enable collisions against [[Static Mesh]]es.
I don't know of a way to enable collisions only with [[Skeletal Mesh]]es,
but we can create a [[Static Mesh]] collision filter that no [[Static Mesh]] will pass.
Add a Component Tag that no [[Static Mesh]] has to the Grid Emitter > Emitter Summary > Collision Data Interfaces > Collision Rigid Mesh Query > Source > Component Tags array.

### Grid 3D FLIP

A FLIP simulation can collide with a [[Skeletal Mesh]] through its [[Physics Asset]].
Enable Emitter node > Emitter Summary > Selection panel > Collisions > User Physics Asset Collisions.
We must tell the simulation which [[Physics Asset]] it should collide with.
Create a USER [[Niagara Parameter]] named PhysicsCollisions of type Physics Asset with System node (the blue one) > User Parameters > + button > Object.
In the Selection panel, rename from NewObject to something descriptive, such as Player Skeleton Mesh.
Set User Parameters > Selection panel > USER | PhysicsCollisions > SOURCE > Mesh User Parameter to Player Skeleton Mesh.

From a [[Blueprint Visual Script]], for example the [[Level Blueprint]], get a reference to an instance of the [[Niagara System]] in the [[Level]].
On the [[Niagara System]] instance call Set Niagara Variable (Object).
Set In Variable Name to Player Skeleton Mesh.
Connect a Skeletal Mesh Component, for example from the Player Pawn, to Object.


## Collision With The Global Distance Field

Unreal Engine keeps a global low-resolution distance field of all static geometry in the scene.
This includes movable [[Static Mesh]]es as long as they don't have Simulate Physics enabled.
The fluid can do collision detection against the global distance field.
Enabled at Grid Emitter > Emitter Summary > Collide Against > Use Global Distance Field Collisions.
The collisions will have low fidelity since the global distance field is low resolution and cannot represent motion.
A [[Static Mesh]] that moves through the fluid will be seen by the fluid as a stationary object that teleports step by step.
This means that is cannot push fluid in front of it correctly.


## Collision With The Depth Buffer

The [[Depth Buffer]] can be used as a very fast collision object.
It forms a 2D surface draping the opaque, i.e. non-[[Translucency|Translucent]], objects from the camera's view-point, forming an in-front-of vs behind relationship for all point within the view frustum in relation to the surface for each pixel. 
The fluid simulations considers all cells behind the surface to be in collision and all cells in front of the surface to not be in collision.
This is enabled at Grid Emitter > Emitter Summary > Collide Against > Use Depth Math Collisions.
Can be combined with Use GBuffer Velocity to read collisions velocities from the [[GBuffer]].

This approach can lead to unexpected fluid flow since any point behind an object is considered to be in collision regardless of how deep that object actually is.


## Collision With The Grid Boundary

The simulation grid boundary can be turned into a wall, preventing the fluid from escaping the simulation.


# Rendering

It is common to render the Density Attribute when rendering dust or smoke.

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

(
Describe when and where the following is shown, and how to toggle it on and off.
)
3D fluid systems are contained withing a simulation domain, a volume in which the fluid exists.
This volume is visualized as a red box, the simulation's bounding box.
The simulation domain is a regular grid of cells, a.k.a. a bunch of stacked boxes.
The size of each cell is shown on each face of the red bounding box.


## Grid Attribute Visualizer

We can visualize grid attributes in the Preview panel.

### Visualize Vector Attribute 

This visualizer renders arrows in the [[Viewport]] based on a vector grid attribute.i
It doesn't render all grid cells at once, that would produce a mess of arrows difficult to interpret.
Instead it renders a slice of the grid.

To a Generic Simulation Stage with an Execute Behavior that runs on tick add a Grid 3D Visualize Vector Field [[Niagara Module]].
Edit the visualizers properties from the Selection panel.
Toggle Debug Draw > Draw Visualizer to enable or disable the visualizer.
By having it disabled most of the time we can have a number of visualizers ready to go whenever we need to.
Set Debug Draw > Grid by clicking the ⌵ button and select > Link Inputs > Emitter > EMITTER | and then the name of your grid in Parameters panel > Emitter Attributes.
Select a grid attribute to visualize: Click Debug Draw > Vector Value > ⌵ button > Link Inputs > Emitter > EMITTER | GRID | attribute name.

The Unit To World parameter tells the visualizer how to convert coordinates in the grid to coordinates in the world so that the rendered arrows shows up at the right place in the [[Viewport]].
It is a matrix and while we can enter numbers manually if we want to,
but a better way is to have a Grid 3D Create Unit To World Transform [[Niagara Module]] in our Emitter, see _Grid Unit Space_, and link the visualizer's Unit To World parameter to Link Inputs > Emitter > EMITTER | GRID 3D CREATE UNIT TO WORLD TRANSFORM | Unit To World.

Press the Play button in the Timeline panel to see the errors in the Preview panel.


# Grid Unit Space

The local, or internal, space of the grid is a 1.0 x 1.0 x 1.0 box ranging from -0.5 to 0.5 on each axis.
Best to keep local and internal separate:
- Internal: The unit cube.
- Local: A regular Unreal  units (cm) coordinate system placed at the grids center.
From time to time we need to transform values in this coordinate system to the world coordinate system.
For example when using Grid 3D Visualize Vector Field [[Niagara Module]], see _Visualize Vector Attribute_.
The operation to go from the grid coordinate system to the world coordinate system is a matrix-vector multiply with a matrix called Unit To World.
The matrix that performs this transform is computed by the Grid 3D Create Unit To World Transform [[Niagara Module]].
The Selection panel for Grid 3D Create Unit To World Transform contains a bunch of grid-related parameters that we need to set so that they match our grid.
- Camera Facing: Don't know what this is.
- Local Pivot: 
- Offset: In world space where the grid origin should be placed. Often ⌵ button > Link Inputs > Engine > ENGINE | OWNER | Position.
	- That is the position of the [[Actor]] that the fluid simulation belongs to.
- Rotation: In world space. Often ⌵ button > Link Inputs > Engine > ENGINE | OWNER | Position.
	- That is the rotation of the [[Actor]] that the fluid simulation belongs to.
- World Grid Extent: The value passed to Grid 3D Set Resolution [[Niagara Module]] > World Grid Extent.
	- It is recommended to make a USER [[Niagara Parameter]] for World Grid Extent that can be used both in Grid 3D Set Resolution and Grid 3D Create Unit To World Transform.

The existence of the Grid 3D Create Unit To World Transform [[Niagara Module]] will cause a bunch of Parameters panel > Emitter Attributes > EMITTER | GRID 3D CREATE  UNIT TO WORLD TRANSFORM parameters to be created.
- Grid Center Position
- Grid Orientation
- Local Pivot
- Local To World
- Unit To World
- World To Local
- World To Unit



# References

- [_Creating Fluid Simulation in UE5 | Inside Unreal_ by Epic Games, 2022 @ youtube.com](https://www.youtube.com/watch?v=k7WLE2kM4po)
- [_Niagara Fluids_ by Daniel Pearson, DevonPenney, Patrick.Kelly @ dev.epicgames.com/community 2022 UE5.2](https://dev.epicgames.com/community/learning/paths/mZ/unreal-engine-niagara-fluids)
- [_Scene Interactions with Niagara Fluids_ by Daniel Pearson @ dev.epicgames.com/community 2022 UE5.0](https://dev.epicgames.com/community/learning/tutorials/8kkP/scene-interactions-with-niagara-fluids)
- [_Niagara Fluids Development_ by Daniel Pearson, Devin Penny @ dev.epicgames.com 2022 UE5.0](https://dev.epicgames.com/community/learning/tutorials/mXXq/niagara-fluids-development)


