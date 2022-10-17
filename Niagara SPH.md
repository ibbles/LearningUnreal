This note outlines the implementation of NiagaraFluid, an SPH implementation on Niagara by mushe.
It is an Unreal Engine 5.0 project.
The project can be cloned from GitHub with
```
/media/s800/UnrealProjects
➤ mkdir NiagaraFluid
➤ cd NiagaraFluid
➤ git clone git@github.com:mushe/NiagaraFluid.git .
```

Opening the project I get a blank / black level.
The log has a few warnings and errors.
A few of them:
```
LogNiagaraShaderCompiler: Warning:
GPU shader compile failed!
Id: 13
Name: FluidSimulationSystem/FluidSimulation/ParticleGPUComputeScript/FNiagaraShader/1

LogNiagaraShaderCompiler: Warning:
/Engine/Generated/NiagaraEmitterInstance.ush:1355:22: error:
reference to local variable 'In__KernelH' declared in enclosing function 'CustomHlsl4ED1007942DA52DC74AF28B48452A234Emitter_AttributeReader_Func_'
                    if (distance >= In__KernelH)
                                    ^
                                    ^
LogNiagaraShaderCompiler: Warning:
/Engine/Generated/NiagaraEmitterInstance.ush:1321:144: note:
'In__KernelH' declared here
        void CustomHlsl4ED1007942DA52DC74AF28B48452A234Emitter_AttributeReader_Func_(int In__N, float3 In__Position, int In__ID, float In__Mass, float In__KernelH, float In__TargetDensity, float In__EosExponent, float In__EosScale, float In__NegativePressureScale, float In__ViscosityCoefficient, float3 In__Velocity, out float Out_OutDensity, out float Out_OutPressure, out float3 Out_OutForce)
                                                                                                                                                       ^
```

Opening Fluid / FluidSimulationSystem I see one [[Niagara Emitter]] with a bunch of of [[Niagara Module|Niagara Modules]].
They all look reasonable to me.
A red warning triangle in the top-right of the Selection panel tells me that there are compiler errors.
But clicking the Compile button gives me a green check-mark and Good To Go. I'm confused.
There are two errors, one in Particle Spawn and one in Particle Update.
Clicking the Show In Niagara Log opens the Niagara Log panel, which says
> System sucessfully compiled.
I'm confused.
How can I find the Module that has the compilation error?
I want to look at a graph that has a red ERROR label under one of the nodes.
I'm gonna open every Module in the Particle Spawn group.
- `InitializeParticle`:
	- I don't see anything wrong, but I'm not sure what to look for.
	- Clicking Compile gives "Script successfully compiled." in the Niagara Log, but we already know that the Niagara Log can't be trusted.
- `Set: (PARTICLES) _Density`:
	- No code, this is a primitive module.
- `Set: (PARTICLES) _Pressure`:
	- No code, this is a primitive module.
- `BoxLocation`:
	- Don't see anything wrong.
	- Can compile successfully.
	- This is an engine module, so I would be surprised if the problem was here.
- `SphereLocation`:
	- Same as `BoxLocation`.
- `Color`:
	- Same as `BoxLocation`.

And that's all modules in Particle Spawn.
I don't know what it thinks the error is.

FluidSimulation > Particle Update > FluidSolver > Accumulate Forces contains a big chunk of custom HLSL code.
This may be where the compiler error printed to the Unreal Editor (as opposed to Niagara) Log.
```
Name: FluidSimulationSystem/FluidSimulation/ParticleGPUComputeScript/FNiagaraShader/1
```

`FluidSimulationSystem` Is the [[Niagara System]].
`FluidSimulation` is the [[Niagara Emitter]].
`ParticleGPUComputeScript` is a generic name for  a [[Niagara Module]] that is run on the GPU.
`FNiagaraShader` I don't know what it means, other than that this particular `ParticleGPUComputeScript` is part of a [[Niagara System]].
`1`. No idea, perhaps just a counter.

```
reference to local variable 'In__KernelH' declared in enclosing function 'CustomHlsl4ED1007942DA52DC74AF28B48452A234Emitter_AttributeReader_Func_'
                    if (distance >= In__KernelH)
                                    ^
```
It says that the code tried to reference `In__KernelH` but it isn't allowed to because that variable is in an enclosing function.
The function is named `CustomHlsl4ED1007942DA52DC74AF28B48452A234Emitter_AttributeReader_Func_`.
That looks like a very auto-generated name.
Not sure what the `AttributeReader` part comes from, but the custom HLSL code in `AccumulateForces` does use an [[Niagara Particle Attribute Reader]].
It does have an input parameter named `KernelH`.
The custom HLSL does contain the following snippet, that includes an expression similar to the one pointed to by the error message:
```c
float KernelSpiky(float distance)
{
    if (distance >= _KernelH) 
    {
        return 0.0;
    } 
    else 
    {
        float x = 1.0 - distance / _KernelH;
        return 15.0 / (_PI * _KernelH3) * x * x * x;
    }
}
```

Not sure why the `_` is added to `KernelH`, or why the error message talkes about `In__KernelH`.
Automatic code rewrite by the Niagara compiler?


I don't know how to take this any further, it seems it cannot run on my machine.

In any case, this example isn't going to answer my main questions, which are how to do efficient collision detection and how to store per-neighbor data.
This example does a loop over all particles in the system for every Module, and only uses Map Set to write from the Modules.


# References

- [NiagaraFluid by mushe @ github.com 2022](https://github.com/mushe/NiagaraFluid)
