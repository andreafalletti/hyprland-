# Foreword
NVIDIA GPUs in ANY capacity are NOT supported. (even if you're using `nouveau`)

Any and all NVIDIA issues will be marked as NVIDIA issues and I will NOT solve them, as I don't own an NVIDIA card.

If, in the future, anyone will want to contribute to get NVIDIA to work well, I'll update this message.

If you want to try nevertheless, try to use the workarounds that work for Sway to run on proprietary NVIDIA. It might work.

Yes, an invisible cursor is an NVIDIA issue that can be fixed with the mentioned above workarounds.

# Installation

Installing Hyprland is very easy. Either you install it from your local package provider (if they provide pkgs for Hyprland) or you install/build it yourself.

**Notice:** Given this project is under development and constantly changing, if you want to keep up to date with the latest commits, please consider updating your packages with `yay -Syu --devel`, or your other preferred package manager.

## Packages
**WARNING:** I do not maintain any packages. If they are broken, try building from source first.

## Arch Linux

*AUR:*
```
hyprland-git - compiles from latest source
hyprland - compiles from latest release source
hyprland-bin - compiled latest release
```

*If you're on Arch Linux, I* ***heavily*** *recommend you use the AUR.*

## Nix

### With flakes
```nix
# flake.nix
{
  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/nixos-unstable";
    hyprland = {
      url = "github:vaxerski/Hyprland";
      # build with your own instance of nixpkgs
      inputs.nixpkgs.follows = "nixpkgs";
    };
  };

  outputs = { self, nixpkgs, hyprland }: {
    nixosConfigurations.HOSTNAME = nixpkgs.lib.nixosSystem {
      # ...
      modules = [
        hyprland.nixosModules.default 
        { programs.hyprland.enable = true; }
        # ...
      ];
    };
  };
```
### Without flakes
```nix
# configuration.nix
{config, pkgs, ...}: let
  flake-compat = builtins.fetchTarball "https://github.com/edolstra/flake-compat/archive/master.tar.gz";
  hyprland = (import flake-compat {
    src = builtins.fetchTarball "https://github.com/vaxerski/Hyprland/archive/master.tar.gz";
  }).defaultNix;
in {
  imports = [
    hyprland.nixosModules.default
  ];

  programs.hyprland.enable = true;
}
```
## Manual (Releases)
Download the most recent release.

copy the binary (Hyprland) to `/usr/bin/`.

copy hyprctl to `/usr/bin/`.

copy the wlroots .so (`wlroots.so.XX032`) to `/usr/lib/`.

copy the desktop entry (`examples/Hyprland.desktop`) to `/usr/share/wayland-sessions/`

the example config is in `examples/Hyprland.conf`.

For updating later on, you can overwrite the binaries (hyprctl, hyprland and libwlroots), you don't need to update anything else.

## Manual (Manual Build)

*Arch dependencies*:

`yay -S gdb ninja gcc cmake libxcb xcb-proto xcb-util xcb-util-keysyms libxfixes libx11 libxcomposite xorg-xinput libxrender pixman wayland-protocols wlroots-git cairo pango`

(If any are missing hmu)

Please note Hyprland builds `wlroots`. Make sure you have the dependencies of wlroots installed, you can make sure you have them by installing wlroots separately (Hyprland doesn't mind)

### CMake (recommended)

then...

```
git clone --recursive https://github.com/vaxerski/Hyprland
cd Hyprland
sudo make install
```

**Note**: Please be advised, that `sudo make install` will overwrite wlroots headers in the `/usr/include` directory. This won't do anything to you if you are using compiled versions of other Wayland compositors (e.g. sway) but will make you unable to compile (/produce broken binaries) if you are planning on compiling e.g. `sway-git`.

To avoid this, you can either:

use Releases, which don't have that issue

OR
```
sudo mv /usr/include/wlr /usr/include/wlrOld
sudo make install
sudo rm -rf /usr/include/wlr
sudo mv /usr/include/wlrOld /usr/include/wlr
```
_(basically, move your headers to wlrOld, build hyprland, remove the hyprland headers and replace them with the old ones)_

### Meson

then...

```
meson _build
ninja -C _build
ninja -C _build install
```

Refer to Debugging to see how to build & debug.

## Crashes at launch

See the log, `cat /tmp/hypr/[INSTANCE_SIGNATURE]/hyprland.log`

*If you are unsure of the signature, grab the one thats the most recently modified.*

Diagnose the issue by what is in the log:
* `sWLRBackend was NULL!` -> launch in the TTY and refer to the wlr logs in RED.
* `Monitor X has NO PREFERRED MODE, and an INVALID one was requested` -> your monitor is bork.
* Other -> see the coredump. Use `coredumpctl`, find the latest one's PID and do `coredumpctl info PID`.
* failing on a driver (e.g. `radeon`) -> try compiling with `make legacyrenderer`, if that doesn't help, report an issue.
* failing on `wlr-xxx` -> try compiling with `make legacyrenderer`, if that doesn't help, report an issue, and also refer to the TTY wlr logs in RED like in the first point.
* failing on `Hyprland` -> report an issue.

## Custom installation (legacy renderer, etc)

cd into the hyprland repo.

for legacy renderer:
```
sudo make clear && sudo make config && make legacyrenderer && sudo cp ./build/Hyprland /usr/bin && sudo cp ./example/hyprland.desktop /usr/share/wayland-sessions
```

_please note the legacy renderer may not support some graphical features._
<br/><br/>
Any other config: (replace [PRESET] with your preset, `release` `debug` `legacyrenderer` `legacyrendererdebug`)
```
sudo make clear && sudo make config && make [PRESET] && sudo cp ./build/Hyprland /usr/bin && sudo cp ./example/hyprland.desktop /usr/share/wayland-sessions
```

## Custom Build flags
To apply custom build flags, you'll have to ditch make.

Supported custom build flags:
```
NO_XWAYLAND - Removes XWayland support
```

How to?

Go to the root repo.

Clean before everything and config the root:
```
make clear && sudo make config
```

Then, configure CMake:
```
mkdir -p build && cmake --no-warn-unused-cli -DCMAKE_BUILD_TYPE:STRING=Release -D<YOUR_FLAG>:STRING=true -H./ -B./build -G Ninja
```
Change `<YOUR_FLAG>` to one of the custom build flags. You **are allowed to** use multiple at once, then add another `-D<YOUR_FLAG_2>:STRING=true`

You can of course also change the `BUILD_TYPE` to `Debug`.

Now, build:
```
cmake --build ./build --config Release --target all -j 10
```
If you configured in `Debug`, change the `--config` to `Debug` as well.

Now, of course, install manually.
```
sudo cp ./build/Hyprland /usr/bin && sudo cp ./example/hyprland.desktop /usr/share/wayland-sessions
```

# Launching
You can launch Hyprland by either going into a TTY and executing `Hyprland`, or with a login manager.

**!IMPORTANT**: Do **not** launch Hyprland with `root` permissions (don't `sudo`)

Login managers are not officially supported, but here's a short compatibility list:
- SDDM → Works flawlessly
- GDM → Works with the caveat of crashing Hyprland on the first launch
- ly → Works with minor issues and/or caveats
