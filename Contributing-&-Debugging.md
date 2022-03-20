# Build in debug

## Recommended, CMake
install the VSCode C/C++ and CMake Tools extensions and use that.

I've attached a launch.json to examples/ that you can copy to your .vscode/ folder in the repo root.

With that, you can build in debug, go to the debugging tab and hit `(gdb) Launch`.

## Custom, CLI
`make debug`

attach and profile in your preferred way.
