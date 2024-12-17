A driver / hardware feature to eliminate on-screen tearing.
It works by holding the next frame to be displayed until the monitor is ready to receive it.
That is, the graphics card only sends one frame at time to the monitor.

In a [[Packaged Project]] it can be enabled with `-VSync` on the command line.
Does not always work, it seems.
To instead disable it, if it is on by default, pass `-NoVSync`.
I'm not sure if these override or are overridden by `.ini` configuration files.
