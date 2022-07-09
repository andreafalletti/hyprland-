### Nothing renders / screen is empty / crash on opening first app
Possible causes:
#### Your themes are not set up properly, making apps crash.
Use something like `qt5ct` (QT) and `lxappearance` (GTK) (*for GTK you can also set up themes with envvars) to set up your themes.

#### Your PC is very, very old.
In that case, see the [Installation Page](https://github.com/hyprwm/Hyprland/wiki/Installation) and try compiling with LEGACY_RENDERER

*For more info about bugs and crashes, see this* *[wiki page](https://github.com/hyprwm/Hyprland/wiki/Crashing-and-bugs)*

### Me cursor no render?
Are you on NVIDIA? If so, then you have been a naughty boy and haven't listened to my tips on other pages. Use the `WLR_NO_HARDWARE_CURSORS=1` envvar.

### How do I screenshot?
Install `grim-git` and `slurp`

Use a keybind (or execute) `grim -g $(slurp)`, select a region. A screenshot will pop into your `~/Pictures/`
(You can configure grim and slurp, see their github pages)

### How do I change my wallpaper?
Install `swaybg`. See its usage with `swaybg --help`

### How heavy is this?
Not that much heavier than Xorg. If you want maximum performance, consider turning off the blur and animations.

### My monitor no worky!
Try changing the mode in your config. If your preferred one doesn't work, try a lower one.
A good way to list all modes is to get `wlr-randr` and do a `wlr-randr --dryrun`

### How do I get a coredump?
*These instructions are ONLY for systemd. If you use anything else, you should know what you're doing.*

Launch `coredumpctl` in a terminal.
Press "END" on the keyboard to go to the end.
Note the **last** (the one furthest to the bottom) crash that has `/usr/bin/Hyprland` as an executable.
Remember the PID of it (the first number after the date in a given line)
exit (Ctrl+C)
type `coredumpctl info PID` where PID is the remembered PID.
Send the entire thing.

### How do I update?
open a terminal where you cloned the repo.
`git pull && sudo make clear && sudo make install`

### Screenshare / OBS no worky!
Yes, hello, this is Wayland.

Amazing gist just about that: https://gist.github.com/PowerBall253/2dea6ddf6974ba4e5d26c3139ffb7580

Please remember in OBS you need to select "Pipewire screen capture" as the source.

### Howdy I screen lock???
Use a wayland-compatible locking utility using WLR protocols, e.g. `swaylock`.

### How do I change me mouse cursor?
Use a tool like for example `lxappearance`. Change your cursor and _restart_ Hyprland. If that doesn't work, change the config files manually according to the [XDG specification (Arch wiki link)](https://wiki.archlinux.org/title/Cursor_themes#Configuration).

Make sure to also edit `~/.config/gtk-4.0/settings.ini` and `~/.gtkrc-2.0` if _not_ using a tool (like `lxappearance`).

Then, do a `gsettings set $gnome-schema cursor-theme 'theme-name'` and you're all good!

### My [program name] is freezing!
Make sure you have a notification daemon running, for example `dunst`. Autostart it with the `exec-once` keyword.

### I want to use Waybar, but the workspaces don't work!
Copy the Waybar source, edit `include/factory.hpp` and add
```
#define HAVE_WLR
#define USE_EXPERIMENTAL
```
on top. If you want to also have sway modules (or any other defined in the file) add the appropriate defines.

All `#ifdef HAVE_<thing>` can be enabled by adding `#define HAVE_<thing>` on top.

Then compile according to the instructions on the Waybar github repo.

Use the `wlr/workspaces` module.

Please do not blame me for this, it's the Waybar dev that doesn't include experimental in the base binary for whatever reason.

### Waybar doesn't show the active workspace!
Use the style for `#workspaces button.active`

### The waybar tray context menus do not show

gtk-layer-shell is not supported by hyprland thus far. In the meantime, you have got to define `#define HAVE_DBUSMENU` in `include/factory.hpp` (see above the FAW about waybar workspaces), and `include "gtk-layer-shell": false` in your waybar config.

### How do I autostart my favorite apps?

Using the window rules to assign apps to workspace you can setup a session start script to open a bunch of applications on various workspaces. The following method will start these apps silently (i.e. without the flickering from workspace to workspace) and deassign the rule so that subsequent start of this app will not start it on the initially assigned workspace (which could be a drag if e.g. you want kitty to be started on ws 1 while you need kitty to open on any workspace subsequently).

Put the following in your hyprdland.conf: (example)
```
windowrule=workspace 1 silent,kitty
windowrule=workspace 1 silent,Subl
windowrule=workspace 3 silent,Mailspring
windowrule=workspace 4 silent,firefox
[...]
exec-once=kitty
exec-once=subl
exec-once=mailspring
exec-once=firefox
[...]
exec-once=cleanup_after_start.sh
```

where `cleanup_after_start.sh` script contains:
```
sleep 10
hyprctl keyword windowrule "workspace unset,kitty"
hyprctl keyword windowrule "workspace unset,Subl"
hyprctl keyword windowrule "workspace unset,Mailspring"
hyprctl keyword windowrule "workspace unset,firefox"
```

in `sleep 10`, the 10 is of course only a suggestion.

### Howdy I move my favorite workspaces to a new monitor when I plug it in?

if you want workspaces to automatically go to a monitor upon connection, use the following:

In hyprland.conf:
```
exec-once=handle_monitor_connect.sh
```

where `handle_monitor_connect.sh` is: (example)
```
#!/bin/sh

function handle {
  if [[ ${1:0:12} == "monitoradded" ]]; then
    hyprctl dispatch moveworkspacetomonitor "1 1"
    hyprctl dispatch moveworkspacetomonitor "2 1"
    hyprctl dispatch moveworkspacetomonitor "4 1"
    hyprctl dispatch moveworkspacetomonitor "5 1"
  fi
}

socat - UNIX-CONNECT:/tmp/hypr/.socket2.sock | while read line; do handle $line; done
```
if you want workspaces 1 2 4 5 to go to monitor 1 when connecting it. 

Please note this requires `socat` to be installed.

### My tablet no worky??
Use [Open Tablet Driver](https://github.com/OpenTabletDriver/OpenTabletDriver) to configure your tablet. In the future it will be supported in the config. Until then, OTD.

### Some of my apps take a really long time to open...?

*~/.config/hypr/hyprland.conf*
```
exec-once=systemctl --user import-environment WAYLAND_DISPLAY XDG_CURRENT_DESKTOP
```

### My cursor in QT apps is too big!
The QT cursor size can be fixed by setting the envvar `XCURSOR_SIZE=24`. You should probably add it to your exported envvars.

### How do I export envvars for Hyprland?
As with any Display Server, Xorg included, you should probably make a script to launch it, for example:

```
export AMONG_US=1
exec Hyprland
```

and launch that. (For Login Manager users, you can replace the "exec" entry in the .desktop file to point to your script)