# Build in debug

## Required packages
`xcb` stuff, check with your local package provider.

`wlroots-git` - always have the latest wlroots.

`wayland` - of course.

*Arch*:

`yay -S gdb ninja gcc cmake libxcb xcb-proto xcb-util xcb-util-keysyms xcb-xfixes x11-xcb xcb-composite xcb-xinput xcb-render pixman wayland-protocols wlroots-git`

(If any are missing hmu)

## Recommended, CMake
install the VSCode C/C++ and CMake Tools extensions and use that.

I've attached a launch.json to examples/ that you can copy to your .vscode/ folder in the repo root.

With that, you can build in debug, go to the debugging tab and hit `(gdb) Launch`.

## Custom, CLI
`make debug`

attach and profile in your preferred way.
