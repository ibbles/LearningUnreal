A Niagara Data Interface is a way to communicate data into a [[Niagara System]].
They are created in the [[Niagara Editor]] and written to from C++ or [[Blueprint Visual Script]].
A Data Interface is a type of [[Niagara Parameter]].
They can be created in the User Parameters panel.
They are also listed in the Parameters panel.
To create a new Data Interface Parameter do User Parameters panel > +-button > Make New > Data Interface > pick a type.
There are Data Interface types for many types of arrays, such as `Bool`, `Color`, `Float`, `Int8`, `Int32`, `Vector`, `Vector2D`, and `Vector4D`.


We typically use `UNiagaraDataInterfaceArrayFunctionLibrary` to manipulate array data.

We write to a Niagara Data Interface using one of the following
- `UNiagaraDataInterfaceArray`
- `UNiagaraDataInterfaceIntRenderTarget2D`
- `UNiagaraDataInterfaceParticleRead`
- `UNiagaraDataInterfaceGrid2D`
- `UNiagaraDataInterfaceGrid3D`
- `UNiagaraDataInterfaceRenderTarget2D`
- `UNiagaraDataInterfaceRenderTarget2DArray`
- `UNiagaraDataInterfaceRenderTargetCube`
- `UNiagaraDataInterfaceRenderTargetVolume`


