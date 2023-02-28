Normally the [[Niagara Module|Modules]] of a [[Niagara Emitter]] group executes all immediately after one another on a per-particle basis.
That is, we cannot use the output from one Module in another Module for another particle.
There can be no cross-communication between particles.
A Simulation Stage introduce a barrier in the Module group.
The modules in one Simulation Stage complete for all particles before the Modules in the next Simulation Stage starts executing.
This means that the second stage can read data written by any particle in the first stage.

A Module in a Simulation Stage can run either over all particles or all elements in a [[Niagara Data Interface]].


There is a built-in performance tool in the [[Niagara Editor]].
Enable Show Runtime Performance For Particle Scripts in the tool bar and the time in ms for each Simulation Stage is shown in the Emitter.
