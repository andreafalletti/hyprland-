# Foreword
NVIDIA GPUs in ANY capacity are NOT supported. (even if you're using `nouveau`)

Any and all NVIDIA issues will be marked as NVIDIA issues and I will NOT solve them, as I don't own an NVIDIA card.

If, in the future, anyone will want to contribute to get NVIDIA to work well, I'll update this message.

If you want to try nevertheless, try to use the workarounds that work for Sway to run on proprietary NVIDIA. It might work.

# Installation

Installing Hyprland is very easy.

## Required packages
`xcb` stuff, check with your local package provider.

`wlroots-git` - always have the latest wlroots.

`wayland` - of course.

*Arch*:

`yay -S gdb ninja gcc cmake libxcb xcb-proto xcb-util xcb-util-keysyms libxfixes libx11 libxcomposite xorg-xinput libxrender pixman wayland-protocols wlroots-git`

(If any are missing hmu)

then...

```
git clone https://github.com/vaxerski/Hyprland
cd Hyprland
sudo make install
```

Refer to Debugging to see how to build & debug.