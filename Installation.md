# Installation

Installing Hyprland is very easy.

## Required packages
`xcb` stuff, check with your local package provider.
`wlroots-git` - always have the latest wlroots.
`wayland` - of course.

*Arch*:

`yay -S gdb ninja gcc cmake libxcb xcb-proto xcb-util xcb-util-keysyms xcb-xfixes x11-xcb xcb-composite xcb-xinput xcb-render pixman wayland-protocols wlroots-git`

(If any are missing hmu)

then...

```
git clone https://github.com/vaxerski/Hyprland
sudo make install
```

Refer to Debugging to see how to build & debug.