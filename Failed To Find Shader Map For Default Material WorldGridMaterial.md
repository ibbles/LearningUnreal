An error that may show up with Unreal Engine 5.5, and possibly later versions.
Is printed when a packaged application is started.
It is not consistent in the sense that it may happen for some scenes but not other.
I have not been able to figure out what triggers it.
Adding or removing usage of the World Grid Material doesn't seem to affect it.

One suggested solution is to enable or disable (don't remember which) Allow Static Lighting in the [[Project Settings]].

Another is to make sure only one [[Rendering Hardware Interface RHI]], or [[Shader Model]] reallly, is selected in the [[Project Settings]].
This can be done programatically by doing text modifications to `Config/DefaultEngine.ini` config file.[]()