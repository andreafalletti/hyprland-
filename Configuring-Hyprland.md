# Configuring

The Hyprland config is very similiar in syntax to the Hypr config. The only difference is that all the variables that didn't have a section are now in the section `general`.

Refer to the example config in `/examples/hyprland.conf` to see, well... and example.

Start a section with `name {` and end in `}` ***in separate lines!***

# Variable types
Variable types are:

`int` - integer

`float` - floating point number

`col` - color (e.g. 0x22334455 - alpha 0x22, red 0x33, green 0x44, blue 0x55)

`MOD` - a string modmask (e.g. SUPER or SUPERSHIFT or SUPERSHIFTALTCTLRCAPSMOD2MOD3MOD5)

Mod list:
```
SHIFT CAPS CTRL/CONTROL ALT MOD2 MOD3 SUPER/WIN/LOGO/MOD4 MOD5
```

# Sections

## General
`max_fps=int` - Maximum refreshes per second. (of config, animations, hyprctl)

`sensitivity=float` - mouse sensitivity

`main_mod=MOD` - the mod used to move/resize floating windows (hold main_mod and mouse1/mouse2)

`border_size=int` - border thickness

`gaps_in=int` - gaps between windows

`gaps_out=int` - gaps between window-monitor edge

`col.active_border=col` - self-explanatory

`col.inactive_border=col` - self-explanatory

# Special keywords

Some keywords arent variables but define special behaviour.

## Monitors
```
monitor=name,res,offset,mfactor,scale```

for example:
```
monitor=DP-1,1920x1080@144,0x0,0.5,1```
will tell Hyprland to make the monitor on DP-1 a 1920x1080 display, at 144Hz, 0x0 off from the beginning, with 0.5 mfactor and a scale of 1.

```
workspace=name,number
```
for example:
```
workspace=DP-1,1
```
will tell Hyprland to make the default workspace on DP-1 a number 1.

## Binds
```
bind=MOD,key,dispatcher,params
```
for example,
```
bind=SUPERSHIFT,Q,exec,firefox
```
will bind opening firefox to SUPER+SHIFT+Q

Dispatcher list:
```
exec - executes a shell command - params: command
killactive - kills the focused window - params: none
workspace - changes the workspace - params: workspace ID of the one to change to
togglefloating - toggles the focused window floating - params: none
```

## Executing
you can execute a shell script on startup of the WM or on each time it's reloaded

`exec-once=command` will execute only on launch

`exec=command` will execute on each reload