#IPC

Hyprland exposes 2 UNIX Sockets:

# /tmp/hypr/.socket.sock

Used for hyprctl-like requests. See the [Hyprctl page](https://github.com/vaxerski/Hyprland/wiki/Using-hyprctl) for commands.

basically, write `command args`.

# /tmp/hypr/.socket2.sock

Used for events. Hyprland will write to each connected client live events like this:

`EVENT>>DATA\n` (\n is a linebreak)

e.g.: `workspace>>2`

## Events list:

### workspace
emitted on workspace change, data is workspace name.