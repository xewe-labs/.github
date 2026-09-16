# Modules

A XeWe OS module is one feature — WiFi, a schedule, a relay — in its own repository. Firmware is
assembled from modules: [`xewe-os`](https://github.com/xewe-labs/xewe-os)'s `setup.sh` reads the
[registry](https://github.com/xewe-labs/xewe-os-modules), copies each chosen module's
`src/<Folder>/` into `src/modules/<Folder>/` and generates `src/modules/Modules.h`, which includes
and declares them in dependency order.

Modules are built on the [XeWeOS framework](https://github.com/xewe-labs/xewe-library-os); its
README ("Writing a module") covers the API and lifecycle, and its `extras/ModuleTemplate` is the
starting point. A module may live under any GitHub account, not only `xewe-labs`.

## Repository layout

| Path | |
|---|---|
| `src/<Name>/<Name>.{h,cpp}` | the module: one folder named like the class |
| `module.properties` | metadata read by `setup.sh` and `scripts/validate.sh` |
| `xewe-os-module-<slug>.ino` | validation firmware: framework + required modules + this module |
| `scripts/validate.sh` | builds that firmware on its own (copied unchanged between modules) |
| `README.md`, `LICENSE.txt`, `.gitignore` | |

Only `src/<Folder>/` is installed into firmware. Everything else — the sketch, scripts, build
folder — exists so the module can be built and tested on its own.

## `module.properties`

```
name=Relay
slug=relay
id=relay
version=0.1.0
description=Switches a relay from the command line and schedules
repo=https://github.com/xewe-labs/xewe-os-module-relay
folder=Relay
include=src/Relay/Relay.h
declare=Relay relay(os, time_module);
depends_modules=time
depends_libraries=XeWeOS (>=0.1.0)
```

* `slug` is lowercase with dashes and names the repository: `xewe-os-module-<slug>`.
* `id` is at most 15 characters. It is the CLI group (`$relay`) and the NVS namespace, so it can
  never change once devices store data under it.
* `folder` is the single folder installed into `src/modules/`, named like the class.
* `declare` is the exact line placed in the generated `Modules.h`. It may use `os` and the variable
  names from the `declare` lines of the modules it requires (`wifi`, `time_module`, ...).
* `depends_modules` lists slugs; setup adds them, and their own requirements, automatically and
  declares them first. `description` is one line: it is what the module checklist shows.

## Code

Global namespace, the class named like the folder, the framework included as `#include <XeWeOS.h>`
and other modules relatively (`#include "../Wifi/Wifi.h"`), since installed modules sit side by
side. The rest is in [`cpp-style.md`](cpp-style.md).

A module takes what it needs by reference and registers the requirement:

```cpp
Relay(xewe::os::ModuleController& controller, Time& time_module, RelayConfig config = {})
    : Module(controller, "relay", "Relay", "Switches a relay",
             /* requires_init_setup */ false,
             /* can_be_disabled     */ true,
             /* has_cli_commands    */ true)
    , time_module(time_module) {
    add_requirement(time_module);
    register_command({"on", "Turn the relay on", "$relay on", 0,
                      [this](std::span<const std::string>) { set(true); }});
}
```

A module whose requirement is disabled is disabled too, and disabling a requirement cascades.

## Validate, then register

```bash
scripts/validate.sh                  # compiles framework + requirements + this module for c3, c6, s3
scripts/validate.sh -b c3 -p <port>  # run it on a board
```

Try it inside firmware before registering, with a local registry list whose entries may be local
folders:

```bash
cp <xewe-os-modules clone>/repositories.txt /tmp/repositories.txt
echo "$HOME/code/xewe-os-module-relay" >> /tmp/repositories.txt
./setup.sh --modules relay --modules-index /tmp/repositories.txt
```

Then open a pull request adding the repository URL to `repositories.txt` in
[xewe-os-modules](https://github.com/xewe-labs/xewe-os-modules); its README lists what reviewers
check. Once merged, the module appears in every `setup.sh` checklist, so treat `main` of a
registered module as something other people build.

## Changing a registered module

`id`, `slug` and `folder` are fixed after registration — devices store settings under `id`, and
firmware includes the folder. Removing or renaming a command changes the interface people script
against: bump MINOR for additions, MAJOR for anything that breaks an existing command, and say so
in the README.
