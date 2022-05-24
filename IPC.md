# IPC

Hyprland exposes 2 UNIX Sockets, for controlling / getting info about Hyprland via code / bash utilities.

# /tmp/hypr/.socket.sock

Used for hyprctl-like requests. See the [Hyprctl page](https://github.com/vaxerski/Hyprland/wiki/Using-hyprctl) for commands.

basically, write `command args`.

# /tmp/hypr/.socket2.sock

Used for events. Hyprland will write to each connected client live events like this:

`EVENT>>DATA\n` (\n is a linebreak)

e.g.: `workspace>>2`

## Events list:

### workspace
emitted on workspace change. Is emitted ONLY when a user requests a workspace change, and is not emitted on mouse movements (see `activemon`)

Data: WORKSPACENAME

### activemon
emitted on the active monitor being changed.

Data: MONNAME,WORKSPACENAME

### activewindow
emitted on the active window being changed.

Data: WINDOWTITLE