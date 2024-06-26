A Niagara Parameter is a piece of data that is held by a [[Niagara System]].
Each parameter has a type, such as float, integer, vector, color (Is this a separate type?), etc.

Each parameter exists within a context.
The contexts match the [[Niagara Namespace]]s.
Parameters high the list are accessible to contexts below it,
but not the other way around.
- System
- Emitter
- Particle

Emitters are aware of System parameters.
Systems are not aware of Emitter parameters.
They can't since there may be many Emitters per System,
which which value should be returned when e.g. EMITTER > Age is referenced?

Some parameters are User Parameters, meaning that they are externally exposed.
Blueprints and C++ code can read (I assume) and write the value held by a parameter.
Such parameters are always associated with the System context.

Create a new user parameter by clicking the + button next to User Parameters in the System node in the [[Node Graph Editor]].

You can see the Parameters in the Parameters panel.
The Parameters panel lists  both user Parameters, called User Exposed, and internal parameters.

The parameters are divided into group:
- User Exposed: Parameters at the [[Niagara System]] level that the user can modify.
	- In the USER [[Niagara Namespace]].
- Emitter Attributes: Parameters that are stored and updated per emitter.
	- In the EMITTER [[Niagara Namespace]].
- Particle Attributes
	- In the PARTICLES [[Niagara Attribute]].

[[Niagara Module]]s also has parameters.

# References

- [_Intro To Niagara_ by Epic Online Learning, James Hill @ dev.epicgames.com/tutorials 2023 UE5.2](https://dev.epicgames.com/community/learning/tutorials/8B1P/unreal-engine-intro-to-niagara)

