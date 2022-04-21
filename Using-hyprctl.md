# Using Hyprctl

`hyprctl` is a utility for controlling some parts of the compositor from a CLI or a script. If you install with `make install`, or any package, it should automatically be installed. 

To check if `hyprctl` is installed, simply execute it by issuing `hyprctl` in the terminal.

If it's not, go to the repo root and `/hyprctl`. Issue a `make all` and then `sudo cp ./hyprctl /usr/bin`.

# Commands

## Control
### Dispatch

issue a `dispatch` to call a keybind dispatcher with an arg.

An arg has to be present, for dispatchers without parameters it can be anything.

Examples:
```
hyprctl dispatch exec kitty

hyprctl dispatch pseudo x
```

## Info

```
monitors - lists all the outputs with their properties
workspaces - lists all workspaces with their properties
clients - lists all windows with their properties
activewindow - gets the active window name
layers - WARNING: Crashes Hyprland often! lists all the layers
```