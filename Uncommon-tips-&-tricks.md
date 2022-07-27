# Uncommon tips & tricks

## Switchable keyboard layouts

An example of a switchable keyboard layout between US and RU, where you switch
between them with SUPER+A (SUPER+Ф)

```
bind=SUPER,A,exec,hyprctl keyword input:kb_layout ru
bind=SUPER,Cyrillic_ef,exec,hyprctl keyword input:kb_layout us
```

You can apply this to any number of languages, mix'n'match, etc.

Please note that if a keyboard layout has a different alphabet, mappings for "a"
"b" "c" will be replaced with mappings from that language. (meaning, e.g.
`SUPER+D` will not work on a `ru` layout, because the russian layout does not
have a `D`.)

If you are unsure about the key names of your chosen alphabet, refer to the
[xkbcommon keysym header](https://github.com/xkbcommon/libxkbcommon/blob/master/include/xkbcommon/xkbcommon-keysyms.h).
The keysym name in Hyprland is the XKB define name without the `XKB_KEY_`.

## Input Method Editors

Currently, the support for IME protocols in wayland is ongoing, but the
protocols are dodgy, not that widely supported, and so on, so it's low priority.

To use IMEs, you can use `ibus` and it will work with all GTK and QT
applications.

Configure ibus just like you would for Xorg.

Then, in your config, make keybind(s) for switching, for example:

```
bind=SUPER,SPACE,exec,bash -c 'if [ "$(ibus engine)" = "anthy" ]; then ibus engine xkb:pl::pol; else ibus engine anthy; fi'
```

will switch on SUPER+SPACE from polish to japanese (anthy)

additionally, to fix the panel closing, use

```
windowrule=nofocus,^(Ibus-ui-gtk3)$
```
