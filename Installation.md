# Foreword
NVIDIA GPUs in ANY capacity are NOT supported. (even if you're using `nouveau`)

Any and all NVIDIA issues will be marked as NVIDIA issues and I will NOT solve them, as I don't own an NVIDIA card.

If, in the future, anyone will want to contribute to get NVIDIA to work well, I'll update this message.

If you want to try nevertheless, try to use the workarounds that work for Sway to run on proprietary NVIDIA. It might work.

Yes, an invisible cursor is an NVIDIA issue that can be fixed with the mentioned above workarounds.

# Installation

Installing Hyprland is very easy. Either you install it from your local package provider (if they provide pkgs for Hyprland) or you build it yourself.

**Notice:** Given this project is under development and constantly changing, if you want to keep up to date with the latest commits, please consider updating your packages with `yay -Syu --devel`, or your other preferred package manager. Always make sure `wlroots` is on the latest commit if you fail to compile.

## Packages
**WARNING:** I do not maintain any packages. If they are broken, try building from source first.

_Arch (AUR)_
```
yay -S hyprland-git
```

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
## Manual

### wlroots
Because we had recently some shenaningans with the AUR package, and so on, here are the instructions for installing wlroots-git manually:
```sh
git clone https://gitlab.freedesktop.org/wlroots/wlroots
cd wlroots
meson build/ --prefix=/usr
ninja -C build/
sudo ninja -C build/ install
```

### Hyprland

*Arch dependencies*:

`yay -S gdb ninja gcc cmake libxcb xcb-proto xcb-util xcb-util-keysyms libxfixes libx11 libxcomposite xorg-xinput libxrender pixman wayland-protocols wlroots-git cairo pango`

(If any are missing hmu)

Please note Hyprland requires `wlroots-git`. Not `wlroots-0.15.1`, not `wlroots-0.16`, `wlroots-git`.

then...

```
git clone https://github.com/vaxerski/Hyprland
cd Hyprland
sudo make install
```

Refer to Debugging to see how to build & debug.

## Crashes at launch

See the `/tmp/hypr/hyprland.log`

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
sudo make clear && make config && make legacyrenderer && sudo cp ./build/Hyprland /usr/bin && sudo cp ./example/hyprland.desktop /usr/share/wayland-sessions
```

_please note the legacy renderer may not support some graphical features._
<br/><br/>
Any other config: (replace [PRESET] with your preset, `release` `debug` `legacyrenderer` `legacyrendererdebug`)
```
sudo make clear && make config && make [PRESET] && sudo cp ./build/Hyprland /usr/bin && sudo cp ./example/hyprland.desktop /usr/share/wayland-sessions
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
make clear && make config
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
- ly → Afaik, it works without issues.
