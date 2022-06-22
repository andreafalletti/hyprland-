# Basic Configuring

For basic syntax info, see [Master Configuring](https://github.com/vaxerski/Hyprland/wiki/Configuring-Hyprland).

This page documents all the "options" of Hyprland. For binds, monitors, execs, curves, etc. see [Advanced Configuring](https://github.com/vaxerski/Hyprland/wiki/Advanced-config).

# Variable types
Variable types are:

`int` - integer

`float` - floating point number

`col` - color (e.g. 0x22334455 - alpha 0x22, red 0x33, green 0x44, blue 0x55)

`MOD` - a string modmask (e.g. SUPER or SUPERSHIFT or SUPERSHIFTALTCTLRCAPSMOD2MOD3MOD5 or empty for none)

Mod list:
```
SHIFT CAPS CTRL/CONTROL ALT MOD2 MOD3 SUPER/WIN/LOGO/MOD4 MOD5
```

# Sections

## General
`max_fps=int` - Maximum refreshes per second. (of config, animations, hyprctl)

`sensitivity=float` - mouse sensitivity

`apply_sens_to_raw=int` - (0/1) if on, will also apply the sensitivity to raw mouse output (e.g. sensitivity in games)

`main_mod=MOD` - the mod used to move/resize floating windows (hold main_mod and mouse1/mouse2)

`border_size=int` - border thickness

`no_border_on_floating=int` - (0/1) disable borders for floating windows

`gaps_in=int` - gaps between windows

`gaps_out=int` - gaps between window-monitor edge

`col.active_border=col` - self-explanatory

`col.inactive_border=col` - self-explanatory

`damage_tracking=str` - Makes the compositor redraw only the needed bits of the display. Saves on resources by not redrawing when not needed. Available modes: `none, monitor, full`. Heavily recommended to use `full` for maximum optimization.

## Decoration

`rounding=int` - rounded corners radius (in pixels)

`multisample_edges=int` - (0/1) enable antialiasing (no-jaggies) for the outsides of rounded corners.

`no_blur_on_oversized=int` - (0/1) disable blur on oversized windows (recommended to leave at default 1, may cause graphical issues)

`active_opacity=float` - self-explanatory, 0 - 1

`inactive_opacity=float` - self-explanatory, 0 - 1

`fullscreen_opacity=float` - self-explanatory, 0 - 1

`blur=int` - (0/1) enable dual kawase window background blur

`blur_size=int` - Minimum 1, blur size (intensity)

`blur_passes=int` - Minimim 1, more passes = more resource intensive.
    
Your blur "amount" is blur_size * blur_passes, but high blur_size (over around 5-ish) will produce artifacts.
    
If you want heavy blur, you need to up the blur_passes.

The more passes, the more you can up the blur_size without noticing artifacts.

`blur_ignore_opacity=int` - (0/1) make the blur layer ignore the opacity of the window.

## Animations

`enabled=int` - (0/1) enable animations

_More about animations is on the Advanced Configuring page._

## Input

`kb_layout=str` `kb_variant=str` `kb_model=str` `kb_options=str` `kb_rules=str` - adequate keyboard settings

`follow_mouse=int` - (0/1/2) enable mouse following (focus on enter new window) - Quirk: will always focus on mouse enter if you're entering a floating window from a tiled one, or vice versa. 0 - disabled, 1 - full, 2 - loose. Loose will focus mouse on other windows on focus but not the keyboard.

`repeat_rate=int` - in ms, the repeat rate for held keys

`repeat_delay=int` - in ms, the repeat delay (grace period) before the spam

`natural_scroll=int` - (0/1) enable natural scroll

`numlock_by_default=int` - (0/1) lock numlock by default

`force_no_accel=int` - (0/1) force no mouse acceleration, bypasses most of your pointer settings to get as raw of a signal as possible.

### Touchpad
_Subcategory input:touchpad:_

`disable_while_typing=int` - (0/1) disable touchpad while typing

`natural_scroll=int` - (0/1) enable natural scroll on touchpads

## Debug

`overlay=int` - (0/1) print the debug performance overlay.

## More 
There are more config options described in other pages, which are layout- or circumstance-specific. See the sidebar navpanel for more pages.
