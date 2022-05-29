# Advanced config

this page documents all of the more advanced config options. Binds, curves, execs, etc.

# Monitors
```
monitor=name,res,offset,scale
```

for example:
```
monitor=DP-1,1920x1080@144,0x0,1
```
will tell Hyprland to make the monitor on DP-1 a 1920x1080 display, at 144Hz, 0x0 off from the beginning and a scale of 1.

Please use the offset for its intended purpose before asking stupid questions about "fixing" monitors being mirrored.

Please remember the offset is calculated with the scaled resolution, meaning if you want your 4K monitor with scale 2 to the left of your 1080p one, you'd use the offset `1920x0` for the second screen. (3840 / 2)

To disable a monitor, use
```
monitor=name,disable
```

If your workflow requires custom reserved area, you can add it with
```
monitor=name,addreserved,TOP,BOTTOM,LEFT,RIGHT
```
Where `TOP` `BOTTOM` `LEFT` `RIGHT` are integers in pixels of the reserved area to add. This does stack on top of the calculated one, (e.g. bars) but you may only use one of these rules per monitor in the config.

```
workspace=name,number
```
for example:
```
workspace=DP-1,1
```
will tell Hyprland to make the default workspace on DP-1 a number 1.

If you want to rotate a monitor, use
```
monitor=NAME,transform,TRANSFORM
```
where `NAME` is the name, and `TRANSFORM` is an integer, from 0 to 7, corresponding to your transform of choice.

***Important!*** This keyword **MUST** be _after_ your `monitor=` keyword with the resolution, etc.
```
WL_OUTPUT_TRANSFORM_NORMAL = 0
WL_OUTPUT_TRANSFORM_90 = 1
WL_OUTPUT_TRANSFORM_180 = 2
WL_OUTPUT_TRANSFORM_270 = 3
WL_OUTPUT_TRANSFORM_FLIPPED = 4
WL_OUTPUT_TRANSFORM_FLIPPED_90 = 5
WL_OUTPUT_TRANSFORM_FLIPPED_180 = 6
WL_OUTPUT_TRANSFORM_FLIPPED_270 = 7
```

# Binds
```
bind=MOD,key,dispatcher,params
```
for example,
```
bind=SUPERSHIFT,Q,exec,firefox
```
will bind opening firefox to SUPER+SHIFT+Q

Please note that `SHIFT` modifies the key names, so for example
```
bind=SHIFT,1,anything,
```
will not work, as 1 is overwritten by !

Common overwrites:
```
1 -> exclam
2 -> at
3 -> numbersign
4 -> dollar
5 -> percent
6 -> asciicircum
7 -> ampersand
8 -> asterisk
9 -> parenleft
0 -> parenright
- -> underscore
= -> plus
```

See the [xkbcommon-keysyms.h header](https://github.com/xkbcommon/libxkbcommon/blob/master/include/xkbcommon/xkbcommon-keysyms.h) for all the keysyms. The name you should use is the one after XKB_KEY_, written in all lowercase.

You can also unbind with `unbind`, e.g.:
```
unbind=SUPER,O
```

May be useful for dynamic keybindings with `hyprctl`.

## General dispatcher list:

### exec
executes a shell command

**params**: command

### killactive
kills the focused window

**params**: none

### workspace 
changes the workspace 

params: workspace (see below)

### movetoworkspace
moves the focused window to workspace X

**params**: workspace (see below)

### movetoworkspacesilent
moves the focused window to workspace X, without changing to that workspace (silent)

**params**: workspace (see below)

### togglefloating
toggles the focused window floating

**params**: none

### fullscreen
toggles the focused window's fullscreen state

**params**: 0 - real fullscreen (takes your entire screen), 1 - "maximize" fullscreen (keeps the gaps and bar(s))

### pseudo
toggles the focused window to be pseudotiled

**params**: none

### movefocus
moves the focus in a specified direction

**params**: l/r/u/d (left right up down)

### movewindow
moves the active window in a specified direction OR monitor

**params**: l/r/u/d (left right up down) OR mon: and ONE OF: l/r/u/d OR name OR id (e.g.: mon:DP-1 or mon:l)

### focusmonitor
focuses a monitor

**params**: ONE OF: l/r/u/d OR name OR id

### splitratio
changes the split ratio

**params**: relative split change, +n/-n, e.g. +0.1 or -0.02, clamps to 0.1 - 1.9

### movecursortocorner
moves the cursor to the corner of the active window

**params**: direction, 0 - 3, bottom left - 0, bottom right - 1, top right - 2, top left - 3.

### workspaceopt
toggles a workspace option for the active workspace.

**params**: a workspace option

Workspace options:
```
allfloat -> makes all new windows floating (also floats/unfloats windows on toggle)
allpseudo -> makes all new windows pseudo (also pseudos/unpseudos on toggle)
```

### exit
exits the compositor. No questions asked.

**params**: none.

## Workspaces
workspace args are unified. You have three choices:

ID: e.g. `1`, `2`, or `3`

Relative ID: e.g. `+1`, `-3` or `+100`

Name: e.g. `name:Web`, `name:Anime` or `name:Better anime`

# Executing
you can execute a shell script on startup of the WM or on each time it's reloaded

`exec-once=command` will execute only on launch

`exec=command` will execute on each reload

# Curves
Defining your own Bezier curve can be done with the `bezier` keyword:

```
bezier=NAME,X0,Y0,X1,Y1
```
where `NAME` is the name, and the rest are two points for the Cubic Bezier. A good website to design your bezier can be found [here, on cssportal.com](https://www.cssportal.com/css-cubic-bezier-generator/).

Example curve:
```
bezier=overshot,0.05,0.9,0.1,1.1
```

# Window Rules
You can set window rules for various actions. These are applied on window open!

```
windowrule=RULE,WINDOW
```

`RULE` is a rule (and a param if applicable) and `WINDOW` is a RegEx to match against

## Rules

### float
floats a window

### tile
tiles a window

### move [x] [y]
moves a floating window (x,y -> int or %, e.g. 20% or 100)

### size [x] [y]
resizes a floating window (x,y -> int or %, e.g. 20% or 100)

### pseudo
pseudotiles a window

### monitor [id]
sets the monitor on which a window should open

### workspace [w]
sets the workspace on which a window should open (for workspace syntax, see binds->workspaces)

### opacity [a]
additional opacity multiplier (a -> float, e.g. 0.25)

_Notice_: Opacity is always a PRODUCT of all opacities. E.g. active_opacity to 0.5 and windowrule opacity to 0.5 will result in a total opacity 0.25. You are allowed to set opacities over 1, but any opacity product over 1 will cause graphical glitches. E.g. 0.5 * 2 = 1, and it will be fine, 0.5 * 4 will cause graphical glitches.

### animation [style] [opt]
forces an animation onto a window, with a selected opt.

e.g.:
```
windowrule=animation slide left,kitty
windowrule=animation popin,dolphin
```

### rounding [x]
forces the application to have X pixels of rounding, ignoring the set default (in `decoration:rounding`)

`x` has to be an int.

## More examples
```
windowrule=float,kitty
windowrule=monitor 0,Firefox
windowrule=move 200 200,Discord
```

# Animations
animations are declared with the `animation` keyword. They can also be declared using a legacy way, but we will not cover that here.

```
animation=NAME,ONOFF,SPEED,CURVE,STYLE
```
for example:
```
animation=workspaces,1,8,default
animation=windows,1,10,myepiccurve,slide
```

`ONOFF` can be either 0 or 1, 0 to disable, 1 to enable.

`SPEED` is the amount of ds (1ds = 100ms) the animation will take

`CURVE` is the bezier curve name, see curves above.

`STYLE` (optional) is the animation style

_Animation names:_
```
windows - window movement/resizing - Styles: slide,popin (fallback is popin)
borders - border color
fadein - fadein/fadeout on window open/close
workspaces - workspace change - Styles: slide,fadein (fadein has currently an issue with borders)
```

# Defining variables
You can define your own custom variables like this:
```
$VAR = value
```

for example:
```
$MyFavoriteGame = Among Us
```

then, to use them, simply use them. For example:
```
col.active_border=$MyColor
```

You ARE allowed to do this:
```
col.active_border=ff$MyRedValue1111
```

# Sourcing (multi-file)
Use the `source` keyword to source another file.

For example, in your `hyprland.conf` you can:
```
source=~/.config/hypr/myColors.conf
```
And Hyprland will enter that file and parse it like a Hyprland config.

Please note it's LINEAR. Meaning lines above the `source=` will be parsed first, then lines inside `~/.config/hypr/myColors.conf`, then lines below.