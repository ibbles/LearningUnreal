A plugin developed by Epic Games to simulate fluids using the [[Niagara]] particle system.
Must be enabled from the [[Plugin|Plugins]] menu.
Both liquids and gasses.
Both 2D and 3D.
Based on grids.
Can also be particle based, the Fluid Simulation Example Content project calls these FLIP simulations.
Since it is an engine plugin, to be able to see the [[Asset]]s included you need to enable [[Content Browser]] > top-right corner > Settings > (Show Engine Content)|(Show Plugin Content).

Everything implemented using the Niagara building blocks.
Built upon [[Niagara Simulation Stage]].
Built upon [[Niagara Grid Collection]] data interface.
A simulation contains multiple grids, each holding one piece of data.
These are called Grid Attributes.
- Density
- Velocity
- Temperature
- Color

A Grid Collection is a collection of grids all of the same size and shape that each holds some piece of data.
We can have a float grid holding densities, a vector grid holding velocities, and another float grid holding temperature.

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


# Parameters

A Niagara Fluid contains parameters at many levels.
Parameters are created with the Set New Or Existing Parameter Directly [[Niagara Module]].

Parameters created in the Emitter Spawn emitter stage in the EMITTER [[Niagara Namespace]] are tied to the emitter as a whole.

Parameters can be included in the Emitter Summary's Selection panel.
This is intended for parameters that we can expect users of the emitter will want to tweak for their specific use-case.
To include a parameter in the Emitter summary select the Set Parameters node that creates it > Selection panel > parameter > right-click > _Emitter Summary_ > Show In Emitter Summary.
Parameters that are commonly shown in the emitter summary:
- World Grid Extents
- Num Cells Max Axis.
- Behavior tweaking parameters for specific [[Niagara Module]]s, such as Curl Noise Force > Noise Strength and Noise Frequency.

The parameters are by default grouped based on the [[Niagara Module]] they come from.
This can be modified by clicking the pen button at the top-right of the Emitter Summary Selection panel to enter edit mode.
There is a ⌵ button next to each parameter and clicking this lets you set
- Display Name
- Category: For example Grid or Simulation.
- Sort Index
- Is Visible

Click the pen button at the top-right of the Selection panel again to leave edit mode.

Click the ⋀ button at the bottom of the emitter node to collapse it down to mostly the Emitter Summary.


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
The full name will be, for example, EMITTER . Grid.

To create the grid object, drag Parameters panel > Emitter Attributes > EMITTER . Grid into the Emitter Spawn stage of the Emitter's node.
This will create a Set: EMITTER . Grid [[Niagara Module]].
Set Selection panel > Set (EMITTER) Grid > EMITTER . Grid > Grid > Num Attributes to 0.
The attributes are created elsewhere.

#### Grid Resolution

To initialize the grid add a Grid 3D Set Resolution [[Niagara Module]] to the Emitter Spawn stage.
Place it after the Set: EMITTER . Grid Module.
To make the Grid 3D Set Resolution Module act on the grid created in the Set: EMITTER . Grid Module, select the Grid 3D Set Resolution Module and click Selection panel > Grid > ⌵ button > Link Inputs > Emitter > EMITTER . Grid.
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
The new parameter will show up at Parameters panel > Emitter Attributes > EMITTER . World Grid Extents.
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
The new parameter will show up at Parameters panel > Emitter Attributes > EMITTER . GRID . Parameter Name.
For modules in a Generic Simulation Stage with the Iteration Source set to a grid Data Interface, STACKCONTEXT and EMITTER . GRID are synonymous.
The parameter can be renamed either from the Parameters panel or from the Set [[Niagara Module]].


### Grid Tick Stage

Create a new simulation stage with Emitter node > + Stage button > Simulation Stages > Generic Simulation Stage.
Give it a name in Generic Simulation Stage Settings > Selection panel > Simulation Stage Name.
Set Simulation Stage > Iteration Source to Data Interface.
Set Data Interface Parameter > Data Interface to EMITTER . Grid.

### Niagara System

Create a new [[Niagara System]] with [[Content Browser]] > right-click > _Create Basic Asset_ > Niagara System.
Select Create Empty System.
Niagara Systems are often named with the `NS_` prefix.
Open the [[Niagara System]] in the [[Niagara Editor]].
System Overview panel > right-click > Add Emitter > Show All > Parent Emitters > select your emitter.
If the emitter was closed in the emitter editor then it is created in its collapsed state, showing mostly the Emitter Summary.
If we expand the emitter in the emitter editor again, then it will expand in the system editor as well.
See _Parameters_ for instructions for how to add specific parameters to the Emitter Summary.

Parameters can be exposed one step further,
making them editable on Niagara System instances in a [[Level]].
This is done by creating a User Parameter for the module parameter.
Emitter Summary > Selection panel > some parameter > ⌵ button > General > Make > Read From New User Parameter.
The new User Parameter shows up in Parameters panel > User Exposed.

# Emitter / Source

In Niagara Fluids an Emitter is also called a Source.
Where regular [[Niagara Emitter]] spawn particles, a fluid simulation Emitter instead writes data into the grid.
The Grid 3D Gas Master Emitter, and possibly others, sources are configured in Selection panel > Sphere Source and Selection panel > Particle Source.

## Sphere Source

There is a sphere source that writes sphere shaped volumes.
Has a position and a scale.
Can write density, temperature, and/or color.

## Particle Source - Other Emitter In Same System

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
Velocity can be bound to PARTICLES . Velocity and Color to PARTICLES . Color.
If you write temperature and/or color in the Set Fluid Source Attributes [[Niagara Module]] then you probably want to enable Grid Emitter > Emitter Summary > Selection panel > Attributes > Density and/or Temperature.


## Data Channels - Emitter In Other System

A Data Channel is a way to feed data into a Niagara Fluid System.

The Set Fluid Source Attributes [[Niagara Module]] **writes** to a data channel.
In Unreal Engine 5.4 this is only supported from emitters with Sim Target set to CPU Sim.
The Set Fluid Source Attributes module should be added to the Particle Update stage.
Enable Set Fluid Source Attributes > Selection panel > Write To Data Channel.

The Set Fluid Source Attributes [[Niagara Module]] can be added to many emitters, potentially in many systems, and they will all write to the same data channel.

To **read** from the data channel you need a Niagara Fluid system created from the 3D Gas template, as of Unreal Engine 5.4. More fluid types may support data channels in later engine versions.
Set 3D Gas Emitter > Selection panel > Particle Source > Particle Source Type to Data Channel.

I don't know how to control which particle systems's data channel the fluid emitter should read from.
There seems to be a single global data channel that everyone shares.


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


# Creating Attributes

Attributes are sometimes called parameters because some parameters can bind to attributes.
Attributes are sometimes called grids because each attribute is stored in its own grid data structure.
All grids together is called a grid collection.
When we say "grid" we often actually mean "grid collection".

An attribute is a named and typed value stored in each cell of the grid.
For example we can have a vector attribute named Velocity and a float attribute named Density.
Attributes are created with the Set New Or Existing Parameter Directly [[Niagara Module]].
The module should be placed in a Simulation Stage with Iteration Source set to Data Interface with a grid as the source and with Execute Behavior set to On Simulation Reset.
When the Set New Or Existing Parameter module is selected the Selection panel lists the parameters that are set when the module is executed.
Any parameter that has the STACKCONTEXT [[Niagara Namespace]] will create a grid attribute since the STACKCONTEXT in this case is the grid, since that is what Iterations Source is set to.

To create a new attribute click Selection panel > Set category > + button > Make New and select the type of the attribute.
- Float
- Vector

By default it will be put in the PARTICLES [[Niagara Namespace]], i.e. one value per particle.
We want one value per grid cell instead, so right-click PARTICLES > Namespace > STACKCONTEXT.
Double-click  the parameter name to change it.
It is common to set the value to 0.0.

# Attribute Index

Each attribute has an index that is unique within the grid.
There is a [[Niagara Dynamic Parameter]] named Grid 3D Get Grid Index From Name that given an attribute name returns the index of that attribute.
It has a parameter named Grid Collection that should be set to the grid to query an attribute index from.
It has a Grid Name attribute that should be set to the name of the attribute to get the index of.
This is a drop-down menu for some reason.
Not sure how to get the attribute index for attributes not in that list.
Or will all attributes be there, populated dynamically based on what attributes we have created?

It can be useful to have Emitter Parameters for each attribute index, for easier access.
In the Emitter Spawn stage create a Set New Or Existing Parameter Directly [[Niagara Module]].
In the Selection panel click the + button and select Make New > Common > int32.
Repeat as many times as you have attributes.
Rename each to the same name as the attribute it belongs to followed by Index.
For example Density Index and Velocity Index.
For each click the ⌵ button > Dynamic Input > Grid 3D Get Grid Index From Name.
Set Grid Collection to the grid.
Set Grid Name to the attribute's name.


# Writing Attributes

## Write All

We can do a blanket write to an attribute in all cells with the Set New Or Existing Parameter Directly [[Niagara Module]].
This is most common in initialization simulation stages.
`In the Selection panel click the + button in the far right of the Set Parameters row.`
Drag Parameters panel > Emitter Attributes > EMITTER . GRID . whatever attribute you want to write to Selection panel > Set Parameters.
In the Selection panel, set the EMITTER . GRID . whatever attribute you want set the value field to the value you want to write.


## Scratch Dynamic Input

We can create our own logic for computing the value set by a Set New Or Existing Parameter Directly,
or any module parameter, I assume.
It is similar to a [[Niagara Scratch Pad Module]], but instead of being a module that goes into an emitter stage it is associated with a particular parameter to a module.
The logic is created with a visual scripting language that has inputs and an output.

If the Simulation Stage that the [[Niagara Module]] is part of has Iteration Source set to Data Interface and bound to a Grid Collection then the Scratch Dynamic Input is executed per grid cell.

To create one that sets a grid cell attribute value, either:
- Drag from Parameters panel > Emitter Attributes > EMITTER . GRID . Attribute Name to one of the simulation stages in an emitter node.
- Click the + button on an emitter stage and select Set New Or Existing Parameter Directly. Click Selection panel > + button and select Link Inputs > Emitter EMITTER . Grid . Attribute Name.

Scratch Dynamic Input works for modules other than Set New Or Existing Parameter Directly as well.

In the Selection panel click the ⌵ button next to a parameter and select New Scratch Dynamic Input.
This will open the Scratch Pad panel.
It starts off containing three nodes:
- Input Map: Provides access to all the parameters this Scratch Dynamic Input has access to.
- Map Get: Similar to [[Niagara Module]]s, the Map Get node takes an Input Map as inputs and reads data from it.
- Output Dynamic Input: The return node. Has a single input pin and whatever we pass to this pin is forwarded to the [[Niagara Module]] parameter that the Scratch Dynamic Input is associated with.

Our job is to decide what inputs we need to read with the Map Get node, do some computation on those values to produce the final result, and connect that result to the Output Dynamic input pin.

The output pins we have on the Map Get node will show up in the Selection panel when the [[Niagara Module]] using the Scratch Dynamic Input is selected.
This is where we assign values to those Map Get output pins.
We can rename Map Get output pins by double-clicking them.

The Map Get node comes with a Default pin that we can remove with right-click > Remove Pin.

To access grid metadata we need a Grid Collection input.
Create one with Map Get > + > Make New > Data Interface > Grid 3D Collection.
With the module using the Scratch Pad Input selected in the emitter node, click the ⌵ button next to the grid parameter and select Link Inputs > Emitter > EMITTER . the name of your grid.
Access to per-cell data is done with parameters of types such as float or Vector.

The Grid parameter is used to find where in the grid the current execution is operating.
That is, it can convert the Execution Index to a Unit space position.
That is, a Vector with 0.0 to 1.0 in all dimensions.
(
Or is it -0.5 to 0.5?
When we set Local Pivot on a 3D Gas Master Emitter then the origin is at the center of the grid bounds.
Is Local Pivot not in Unit space but some other unit space?
)
This is done with the Execution Index To Unit function.

When operating on grid cell positions it is common to use the Unit To World transform.
Get access to it by creating a Matrix pin on the Map Get node.
With the module using the Scratch Dynamic Input selected do Selection panel > Unit To World parameter > ⌵ button > Link Inputs > Emitter > EMITTER . GRID 3D CREATE UNIT TO WORLD TRANSFORM . Unit To World.
For this to show up you need a Grid 3D Create Unit To World Transform module in the Emitter Spawn stage.

To convert the Unit position we get from Grid > Execution Index To Unit to a world space position use the Unit To World matrix input and Matrix Transform Position.
I don't know what the difference, if any, between Transform Position and Matrix Transform Position is.
Execution Index To Unit returns a Vector, so it must be converted to a Position before it can be connected to Matrix Transform Position.

To read grid cell data create a Map Get pin of the type of data you want to read. So, for example, for density you would select float and for velocity you would select Vector.
Select System Overview panel > Emitter node > the module with a Scratch Dynamic Input.
The Map Get output pins we created in the Scratch Dynamic Input is listed in the Selection panel.
Next to the per-cell data parameter click the ⌵button and select > Link Inputs > Emitter > EMITTER . GRID . some grid attribute.


### Custom HLSL

A Scratch Dynamic Input can include HLSL code snippets.
Create a Custom HLSL node with right-click > Functions > Custom HLSL.
The Custom HLSL node contains a text box where we write our HLSL code.
A Custom HLSL node can have input and output pins.
There are accessible as variables in the HLSL code.
Drag a wire from an output pin to an empty input pin on the Custom HLSL node to create an input variable of the same type as the connected output pin..
Double-click the input pin name to rename both the pin and the variable.
You can also create an input variable by clicking the + button next to an empty input pin and select a type.
Output pins are created in the same way.
Example HLSL code for a Custom HLSL node that has a World input of type Position and a Density output of type float, that writes a small density to a sphere with radius 20 cm around the world origin:
```hlsl
Density = length(World) < 20 ? 0.1 : 0.0;
```

## Sphere Source

This is set of properties at Grid Node > Emitter Summary > Selection panel > Sphere Source.


## Particle Source Reader - Reading From A Particle System

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
	- ⌵ button > Link Inputs > Emitter > EMITTER . Grid, or whatever you named your Emitter Parameter for the grid to.
- Unit To World: Transformation matrix converting grid internal space coordinates to world space coordinates.
	- See _Grid Unit Space_ and then ⌵button > Link Inputs > Emitter > EMITTER . GRID 3D CREATE UNIT TO WORLD TRANSFORM > Unit To World.

The output from such modules are stored somewhere where they can be linked to from other module parameters.
In the Curl Noise Force example the output can be linked to with ⌵ button > Link Inputs > Output > OUTPUT . CURL NOISE FORCE . Curl Nose Force.

# Simulation

A Niagara Fluid simulation is split up into stages.
The stages are not fixed, each emitter can have its own set of stages.
Common stages are:
- Init: Run once when the Niagara Fluid is spawned.
- Pre Simulation: Run once each tick.
- Simulation: Run one or more times each tick.
- Post Simulation: Run once each tick.

We split the per-tick part up because some fluid simulation algorithms need multiple iterations to find the next state and there may be some work that don't need to be run multiple times.
Put such work either in Pre Sim or Post Sim.

Create a new stage with Emitter node > + Stage button at the top-right > Generic Simulation Stage.
This creates a new group in the Emitter node with a Generic Simulation Stage Settings child.
When Generic Simulation Stage Settings is selected the Selection panel shows properties for the stage.
- Simulation Stage Name: The name of the stage.
	- Is shown in the Emitter node.
- Enable Binding: Don't know yet.
- Num Iterations: The number of times the [[Niagara Module]]s in the stage are executed per tick.
	- Set this to 1 for the Pre Sim and Post Sim stages, and however many iterations you need for the Sim stage.
- Num Iterations Bindings: Don't know yet.
- Iteration Source: This controls if the stage simulates particles or something else, such as a grid.
	- For a grid simulation, set Iteration Source to Data Interface. This causes the Data Interface Parameters category to show up in the Selection Panel. Set Data Interface Parameters > Data Interface to your grid, i.e. EMITTER . your grid.

## Pre Simulation Stage


### Buoyancy

A simple buoyancy-like effect can be made by adding a velocity to all cells in the grid.
This should be done in the pre simulation stage because it is not something that is iterated but set directly.


## Simulation Stage

A bunch of simulation-related [[Niagara Module]]s are included with the engine.
You can find many of them under the Grid 3D group that opens when clicking the + button next to the simulation stage name.
Some, such as Dissipate Float, are found under Grid 2D.
Not sure how to determine if a Grid 2D [[Niagara Module]] also works with a 3D grid.

The output from one module can be passed to the input of another.
To do this bind the parameter of the second module to the write parameter of the first module.
The write parameters are hidden in the Selection panel by default.
To unhide them, with a [[Niagara Module]] selected select Selection panel > dashed down arrow button > Show Parameter Writes to see where the output of a particular module is written.
This is often not back to the grid cell value that was read,
but instead to some temporary storage.
Often named OUTPUT . MODULE NAME . Output Value Name.
To write the value back to the grid use a Set New Or Existing Parameter [[Niagara Module]].
With the Set Parameters module selected, drag the Emitter Attribute you want to write to from the Parameters panel to the Set Parameters row in the Selection panel.
It will show up as a parameter to the Set Parameters module.
Bind that parameter to Link Inputs > Output > OUTPUT . MODULE NAME > Output Value Name.

If you add another [[Niagara Module]] at the end of a sequence that computes a new grid attribute value, remember to update the Set Parameter module to read from the new last module's output.
If you forget this the module will compute its result but it will never be used for anything.

### Grid 3D Advect Scalar

Move values in the grid cells according to the velocities in the grid cells. Does not write the updated grid values back to the same grid attribute, but to OUTPUT . GRID 3D ADVECT SCALAR . Advected Scalar.

Parameters:
- Advection Method: Don't know yet.
- Advected Grid: The grid that contains the scalar attribute to be advected. Set with ⌵ button > Link Inputs > Emitter > EMITTER  . your grid.
- dt: The delta time for the current tick. ⌵ button > Link Inputs > Engine > ENGINE . Delta Time.
- dx: The size of a cell. Typically bound to an Emitter Parameter that has been initialized to EMITTER . GRID 3D SET RESOLUTION . World Cell Size > X, which is created by the Grid 3D Set Resolution [[Niagara Module]].
- Scalar Index: The attribute index of the grid attribute to advect. We typically create Emitter Parameters for these, see _Attribute Index_.
- Velocity Grid: The grid that contains the velocity attribute causing the advection. Often the same as Advected Grid.
- Velocity Index: The attribute index of the velocity grid attribute. See _Attribute Index_.


### Dissipate Float

This one is in Grid 2D, not Grid 3D.
Seems to work with 3D grids anyway.
I assume because it doesn't use the grid structure for anything, it just reads one single float and it doesn't matter where it comes from, the computation is the same regardless.

Parameters:
- Dissipation Rate: How fast to dissipate. Don't know the unit of this. Values around 1.0 seems to work well.
- dt: Bind to Link Inputs > Engine > ENGINE . Delta Time.
- Float Value: The value to dissipate.
	- Bind it to e.g. Link Inputs > Emitter > EMITTER . GRID . Density, or, if you have another module, such as Grid 3D Advect Scalar, that you want to dissipate the output of, bind the Float Value parameter to Link Inputs > Output > OUTPUT . MODULE NAME . Output Name, for example OUTPUT . GRID 3D ADVECT SCALAR . Advected Scalar.


## Post Simulation Stage

Generate lighting and rendering data needed by the [[Material]] used to render the Niagara Fluid.
See _Rendering_.
These [[Niagara Module]]s generate data used by the `M_RayMarch_Smoke_inst` [[Material]].

### Grid 3D Bake Directional Light

Generate light data for a directional light.
Should be placed before the Grid 3D Set RT Values [[Niagara Module]] since the values computed by Grid 3D Bake Directional Light will be written into the [[Volume Texture]] by Grid 3D Set RT Values.

Parameters:
- Iteration Grid: Our grid. Not sure what the Iteration part is for.
- Grid: Also our iteration grid. Don't know why we need both.
- Density Index: The _Attribute Index_ of the density attribute.
- Density Mult: Scalar to scale the density value up or down.
	- Setting this smaller than 1.0 makes the light illuminate deeper into the smoke.
- Light Intensity: How strong the light is.
- dx: The size of one grid cell in cm. Bind this to an Emitter parameter that in turn is bound to EMITTER . GRID 3D SET RESOLUTION . World Cell Size . X.
- World Grid Extends: The size of the grid in world space. Bind this to an Emitter parameter in that is also used by the Grid 3D Set Resolution [[Niagara Module]].
- World To Local: Transformation matrix that transforms from the world space to the grid's local, not internal, space. Bind to EMITTER . GRID 3D CREATE UNIT TO WORLD TRANSFORM . World To Local.
- Unit To World: Transformation matrix that transforms from the unit space to the world space. Bind to EMITTER . GRID 3D CREATE  UNIT TO WORLD TRANSFORM . Unit To World.

If Grid 3D Bake Directional Light > Selection panel > dashed down arrow > Show Parameter Writes is enabled then under Parameter Writes we can see that this module writes its output to OUTPUT. GRID 3D BAKE DIRECTIONAL LIGHT > Transmittance.

The `M_RayMarch_Smoke_Inst` [[Material]] expects to find this value in the green channel of the [[Volume Texture]].
Grid 3D Set RT Values [[Niagara Module]] > Selection panel > Green > ⌵ button > Link Inputs > Output > OUTPUT . GRID 3D BAKE DIRECTIONAL LIGHT > Transmittance.

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
Set User Parameters > Selection panel > USER . PhysicsCollisions > SOURCE > Mesh User Parameter to Player Skeleton Mesh.

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

## Smoke

It is common to render the density attribute when rendering dust or smoke.
In order to communicate the density field from the Niagara grid to a render material we need to pass it through a [[Volume Texture]].
That [[Volume Texture]] must be created and initialized to have the same size as the grid.
Create a new parameter with Parameters > Emitter Attributes > + button > Make New > Data Interface > Render Target Volume.
Name it Density Render Target.
Drag the parameter from the Parameters panel to the Emitter Spawn stage to create a Set Parameter [[Niagara Module]], leave all Selection panel settings at their defaults.
This will create the [[Volume Texture]] as a 4-channel RGBA texture but not initialize it.
Add a Grid 3D Init RT [[Niagara Module]] after the Set Parameter module.
Set Grid 3D Init RT > Selection panel > RT to Link Inputs > Emitter > EMITTER . Density Render Target.
Set Grid 3D Init RT > Selection panel > Grid to Link Inputs > Emitter > EMITTER . Grid, or whatever your grid collection is named.
This will resize the [[Volume Texture]] to match the size of the grid.
(
How do we control if the [[Volume Texture]] is a scalar texture or a vector texture?
Is it always a vector texture?
)

The [[Volume Texture]] is typically populated in a simulation stage that is run after the update simulation stage, often named Post Sim or Post Simulation.
Create a new simulation stage with emitter node > + stage button > Simulation Stages > Generic Simulation Stage.
In Generic Simulation Stage Settings > Selection panel set
- Simulation Stage Name to Post Simulation.
- Iteration Source to Data Interface.
- Data Interface to EMITTER . Grid, or whatever you named your grid.

To the Post Sim simulation stage add a Grid 3D > Grid 3D Set RT Values [[Niagara Module]].
In Grid 3D Set RT Values > Selection panel set
- Grid to Link Inputs > Emitter > EMITTER . Grid, or whatever  you named the grid to.
- RT to Link Inputs > Emitter > EMITTER . Density Render Target.

The rest of the parameters on Grid 3D Set RT Values is what to search each channel of the [[Volume Texture]] to.
What these should be depends on the [[Material]] that reads from the [[Volume Texture]].
Described here is the setup expected by the `M_RayMarch_Smoke_Inst` [[Material]], which is included with the engine.
- Red: Density.
- Green: Transmittance.
- Blue: Don't know yet.
- Alpha: Don't know yet.

The red channel is used for density.
To write the density attribute to the red channel set Red to Link Inputs > Emitter > EMITTER . GRID . Density.

The green channel is used for lighting information.
You can set that to 1.0 to make the smoke gray instead of pure black.
For better lighting add either a Grid 3D Bake Directional Light or a Grid 3D Bake Point Light [[Niagara Module]] to the Post Simulation stage,
before the Grid 3D Set RT Values [[Niagara Module]].
On the Grid3D Set RT Values module, set Green to Link Inputs > Output > OUTPUT. GRID 3D BAKE (POINT)|(DIRECTIONAL) LIGHT . Transmittance.
See _Grid 3D Bake (Point)|(Directional) Light_ under _Post Simulation Stage_.

The [[Material]] needs to know how large each grid cells is.
It assumes that each grid is a cube, i.e. with equal sides, so it wants a float.
The Grid 3D Set Resolution [[Niagara Module]] computed the cell size for us, but stored it in a Vector parameter, we need it in a float parameter.
Create an emitter parameter for the float.
Emitter node > Emitter Spawn stage > + button > Set New Or Existing Parameter Directly.
Selection panel > Set Parameters > + button > Make New > Common > float.
Name  it dx.
dx > ⌵ button > General > Dynamic Inputs > Make Float From Vector.
Vector > ⌵ button > Link Inputs > Emitter > EMITTER . GRID 3D SET RESOLUTION . World Cell Size.
Leave Channel set to X.

Rendering [[Niagara Module]]s are added to the Render stage in the emitter node.
Create a Mesh Renderer to render smoke.
Set Mesh Renderer >Selection panel > Mesh Rendering > Source Mode to Emitter.
This is to not create one mesh per particle, since we don't have any particles, but instead a single mesh for the entire emitter.
Set Mesh Renderer > Selection panel > Mesh Rendering > Meshes > element 0 to `s_cube_1cm_flippednormals`.
This is a [[Static Mesh]] that has been designed specifically to render volume data such as the density field of a smoke simulation.
Set the neighboring Enable Material Overrides.
Add an array element to the neighboring Material Overrides array.
In the array element set Explicit Mat to `M_RayMarch_Smoke_Inst`.
This is a [[Material]] that does ray marching into volume data.
A few bindings need to be setup in Mesh Renderer > Selection Panel > Bindings:
- Position Binding: The center of the grid in world space.
	- Bind this to a Position emitter parameter named Grid Center set in Emitter Update that in turn is bound to Convert Vector To Position that in turn is bound to Link Inputs > Emitter > EMITTER . GRID 3D CREATE  UNIT TO WORLD TRANSFORM > Grid Center Position. Require that a Grid 3D Create Unit To World Transform [[Niagara Module]] has been added to the Emitter Update stage before the Set Parameter module setting the Grid Center parameter.
- Scale Binding: EMITTER . GRID 3D SET RESOLUTION . World Grid Extents.
- Material Parameter Bindings: Add the following array elements:
	- 0:
		- Material Parameter Name: Volume Tex.
		- Niagara Variable: Emitter > EMITTER . Density Render Target.
		- This informs the [[Material]] of which [[Volume Texture]] that the emitter writes densities to.
	- 1:
		- Material Parameter Name: Voxel Size.
		- Niagara Variable: Emitter > EMITTER . dx.
	- 2:
		- Material Parameter Name: World Grid Extents.
		- Niagara Variable: Emitter > EMITTER . World Grid Extents.
	- 3-6:
		- Material Parameter Name: World To Local \[0-3\].
		- Niagara Variable: Emitter > EMITTER. GRID 3D CREATE UNIT TO WORLD TRANSFORM . World To Local \[0-3\].


## Lighting And Shadows

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

This visualizer renders arrows in the [[Viewport]] based on a vector grid attribute.
It doesn't render all grid cells at once, that would produce a mess of arrows difficult to interpret.
Instead it renders a slice of the grid.

To a Generic Simulation Stage with an Execute Behavior that runs on tick add a Grid 3D Visualize Vector Field [[Niagara Module]].
Edit the visualizers properties from the Selection panel.
Toggle Debug Draw > Draw Visualizer to enable or disable the visualizer.
By having it disabled most of the time we can have a number of visualizers ready to go whenever we need to.
Set Debug Draw > Grid by clicking the ⌵ button and select > Link Inputs > Emitter > EMITTER . and then the name of your grid in Parameters panel > Emitter Attributes.
Select a grid attribute to visualize: Click Debug Draw > Vector Value > ⌵ button > Link Inputs > Emitter > EMITTER . GRID . attribute name.
I don't know why, but we must also set Vector Index to the attribute index of the attribute selected for Vector Value.
Either use a Grid 3D Get Grid Index From Name [[Niagara Dynamic Parameter]] or set to Link Inputs > Emitter > EMITTER . attribute name, if you have created such a emitter parameter.
See _Attribute Index_ for details.
The Plane parameter is used to control which subset of data from the grid to visualize.

The Unit To World parameter tells the visualizer how to convert coordinates in the grid to coordinates in the world so that the rendered arrows shows up at the right place in the [[Viewport]].
It is a matrix and while we can enter numbers manually if we want to,
but a better way is to have a Grid 3D Create Unit To World Transform [[Niagara Module]] in our Emitter, see _Grid Unit Space_, and link the visualizer's Unit To World parameter to Link Inputs > Emitter > EMITTER . GRID 3D CREATE UNIT TO WORLD TRANSFORM . Unit To World.

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
- Offset: In world space where the grid origin should be placed. Often ⌵ button > Link Inputs > Engine > ENGINE . OWNER . Position.
	- That is the position of the [[Actor]] that the fluid simulation belongs to.
- Rotation: In world space. Often ⌵ button > Link Inputs > Engine > ENGINE . OWNER . Position.
	- That is the rotation of the [[Actor]] that the fluid simulation belongs to.
- World Grid Extent: The value passed to Grid 3D Set Resolution [[Niagara Module]] > World Grid Extent.
	- It is recommended to make a USER [[Niagara Parameter]] for World Grid Extent that can be used both in Grid 3D Set Resolution and Grid 3D Create Unit To World Transform.

The existence of the Grid 3D Create Unit To World Transform [[Niagara Module]] will cause a bunch of Parameters panel > Emitter Attributes > EMITTER . GRID 3D CREATE  UNIT TO WORLD TRANSFORM parameters to be created.
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
- [_Source Into Fluids From Any Niagara System (Data Channels)_ by Daniel Pearson @ dev.epicgames.com/tutorials 2024 UE5.4](https://dev.epicgames.com/community/learning/tutorials/7JDb/unreal-engine-source-into-fluids-from-any-niagara-system-data-channels)


