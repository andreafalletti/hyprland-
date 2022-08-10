# Status Bars

## Waybar

Waybar is a GTK status bar made specifically for wlroots compositors.

To use it, it's recommended to either use the AUR package `waybar-hyprland-git`, or compile manually with the `USE_EXPERIMENTAL` flag enabled.

To compile manually:

Clone the source, then edit `include/factory.hpp` and add

```
#define HAVE_WLR
#define USE_EXPERIMENTAL
```

on top. If you want to also have sway modules (or any other defined in the file)
add the appropriate defines.

All `#ifdef HAVE_<thing>` can be enabled by adding `#define HAVE_<thing>` on
top.

Then compile according to the instructions on the Waybar github repo.

If you want to use the workspaces module, it's called `wlr/workspaces`.

For more info regarding configuration, see [The Waybar Wiki](https://github.com/Alexays/Waybar/wiki)

## EWW

In order to use [eww](https://github.com/elkowar/eww), you first have to
install it, either using your distro's package manager, by searching
`eww-wayland`, or by manually compiling. In the latter case, you need to
have `cargo` and `rustc` installed, then clone the repo and run `cargo build`.

### Configuration

After you've successfully installed eww, you can move onto configuring it.
There are a few examples listed in the [Readme](https://github.com/elkowar/eww),
we highly recommend you to also read through the
[Configuration options](https://elkowar.github.io/eww/configuration.html).

**NOTE:** Read
[the wayland section](https://elkowar.github.io/eww/configuration.html#wayland)
carefully before asking why your bar doesn't work.