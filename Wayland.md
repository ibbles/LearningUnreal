Unreal has some trouble with Wayland, at least in the Unreal 5.5 to 5.7 time frame.

[Brick Frog @ discord.com](https://discord.com/channels/187217643009212416/375022233875382274/1513100787683819540):
> You have two primary options with Ubuntu 26.04 - attempt to use XWayland for things, which may or may not work well for you; or install a separate DE
>
> There is literally no support in GNOME anymore for xorg/x11 desktop sessions. I personally just installed xubuntu-desktop since I do like XFCE. If you want to go with KDE, you can very simply install the plasma-session-x11 package.
>
> You can just install xubuntu/kubuntu/lubuntu whatever flavor you want and you can use x11 with ease. Just not with GNOME. The different flavors (xubuntu, etc) all have LTS versions that are supported for ~3 years.
>
>  You can keep GNOME installed with a regular Ubuntu install and just install one of the other desktop environments and toggle between them. You'll likely want to change which display manager you're using if you're going to be mostly running another DE, but you can keep GNOME and all of its packages installed and switch back and forth if you need to


[NathanaelA @ discord.com](https://discord.com/channels/187217643009212416/375022233875382274/1505986808528244819)
> From reports I've seen 5.8 is much better on XWayland than before. You are still recommended to use `SDL_VIDEO_DRIVER=x11` to force XWayland. Personally if your distro supports it, just use X11 and you will have very few issues. But it seems like their is movement forward for those who use Wayland...


[Brick Frog @ discord.com](https://discord.com/channels/187217643009212416/375022233875382274/1502013501382594711)
> ubuntu 26.04 removed the Xorg session option from the login screen, which used to use X11 for everything. it only has Wayland out of the box now, but still allows Xwayland. I haven't tried to use just Xwayland to run both Rider and UE5, but all wayland support is currently quite broken in unreal so you might want to stick with 24.04 or use Xubuntu.


