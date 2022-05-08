### How do I screenshot?
Install `grim-git` and `slurp`

Use a keybind (or execute) `grim -g $(slurp)`, select a region. A screenshot will pop into your `~/Pictures/`
(You can configure grim and slurp, see their github pages)

### How do I change my wallpaper?
Install `swaybg`. See its usage with `swaybg --help`

### How heavy is this?
There are two things that impact Hyprland's amount of GPU taxing right now:

minor - animations

major - full damage tracking being wonky

Once full damage tracking is fixed, yes, it will be heavier than e.g. Sway, because we have animations, blur, fancy effects, et cetera, but it shouldn't be that much worse than say, guhnome.

### My monitor no worky!
Try changing the mode in your config. If your preferred one doesn't work, try a lower one.
A good way to list all modes is to get `wlr-randr` and do a `wlr-randr --dryrun`

### I crash on start!
Launch from a TTY.
If you still crash, then:
  -> if you get red logs, google them, they are wlr errors. If that doesn't help, make an issue or post on the discord server.
  -> if you don't, make an issue (/post on the discord server) and ***attach*** (don't use pastebin/paste it) the log, `/tmp/hypr/hyprland.log` AND a coredump.

### I crash not on start!
 make an issue / post on the discord server, explain what you were doing, and ***attach*** (don't use pastebin/paste it) the log, `/tmp/hypr/hyprland.log` AND a coredump.

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

Switch to Pipewire if you haven't done so yet.

Install `wireplumber`

Install `xdg-desktop-portal` and `xdg-desktop-portal-wlr`

reboot

Should be working now.

If it doesn't, disable the services provided by both xdg portals (`systemctl --user disable name`), make a .sh file somewhere with:
```
#!/bin/bash
sleep 4
killall xdg-desktop-portal-wlr
killall xdg-desktop-portal
/usr/lib/xdg-desktop-portal-wlr &
sleep 4
/usr/lib/xdg-desktop-portal &
```
do a `chmod +x thefile`, and then, in your Hyprland config use `exec-once` to exec it on start.

Please remember in OBS you need to select "Pipewire screen capture" as the source.

### Howdy I screen lock???
Use a wayland-compatible locking utility using WLR protocols, e.g. `swaylock`.

### GTK apps' borders are weird? and mouse is off!
Go to your GTK theme's folder, then to `gtk-4.0`. In it, you'll find at least one CSS file. For all the CSS files, search for `window {` and in the window class, replace all `border` and `box-shadow` properties to `none`.

### How do I change me mouse cursor?
Use a tool like for example `lxappearance`. Change your cursor and _restart_ Hyprland. If that doesn't work, change the config files manually according to the [XDG specification (Arch wiki link)](https://wiki.archlinux.org/title/Cursor_themes#Configuration).

Make sure to also edit `~/.config/gtk-4.0/settings.ini` and `~/.gtkrc-2.0` if _not_ using a tool (like `lxappearance`).

Then, do a `gsettings set $gnome-schema cursor-theme 'theme-name'` and you're all good!

### My <program name> is freezing!
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

Please do not blame me for this, it's the Waybar dev that doesn't include experimental in the base binary for whatever reason.

### Waybar doesn't show the active workspace!
Use the style for `#workspaces button.active`
