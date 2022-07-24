# Texture Streaming Pool

Unreal Engine allocates a limited amount of VRAM for texture streaming.
If we add too many different texture-using assets to our scene then we will fill that amount.
There is a [[CVars|CVar]] to control the size of the pool.
```
r.Streaming.PoolSize
```
This measures the size of the texture streaming pool in MiB.
By default it is 1024 (I think).
If you have VRAM to spare then you can increase it.
```
r.Streaming.PoolSize 3000
```

# References

- [_Managing the Texture Streaming Pool | Tips & Tricks | Unreal Engine_ by Unreal Engine @ youtube.com, 2022](https://youtu.be/uk3W8Zhahdg)

