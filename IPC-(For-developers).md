# IPC

You can easily talk to the Hyprland compositor and request information from it.

The current implementation uses sockets to open and close a connection.

For reference, see the `hyprctl` utility located in this repo in the `hyprctl/` directory.


## Socket info

The socket is created in `localhost` and the port of the socket will be located in `/tmp/hypr/.socket`

*Example contents of the* `/tmp/hypr/.socket` *file:*
```
1337
```

To connect, simply connect as you would with a socket to the port.

If you don't know how to open/read/whatever, I recommend this brilliant C/C++ tutorial for sockets: [linuxhowtos.org/C_C++/socket.htm](https://www.linuxhowtos.org/C_C++/socket.htm)

## Rules of communication

Once Hyprland accepts your connection, it is your job to `write()` a request. After that, `read()` the reply.

### Accepted requests:
```
monitors
clients
workspaces
```

Explanations:

`monitors` - Self-explanatory: retrieves all monitors' info

`clients` - Self-explanatory: retrieves all clients' info

`workspaces` - Self-explanatory: retrieves all workspaces' info