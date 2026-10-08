# XeWe Labs

XeWe is a small ecosystem for ESP32-C3, -C6 and -S3 firmware that you control from a command
line, over serial or over the network. You clone one template, pick modules, and build with one
local tool. A module is a piece of code someone (often a coding agent) figured out once, so nobody
has to figure it out again. Humans and agents use the same commands, and everything still works
with no board attached: builds compile and tests report "compiled, not run".

## Start here

Clone [**xewe-os**](https://github.com/xewe-labs/xewe-os), then `./setup.sh` and `./run.sh`. Its
[README](https://github.com/xewe-labs/xewe-os#readme) covers the first five minutes.
[ARCHITECTURE.md](https://github.com/xewe-labs/xewe-os/blob/main/ARCHITECTURE.md) explains why it
is shaped this way.

## XeWe OS

| Repository | |
|---|---|
| [`xewe-os`](https://github.com/xewe-labs/xewe-os) | the firmware template you clone (or "Use this template"); holds only your code and `xewe.lock` |
| [`xewe-os-core`](https://github.com/xewe-labs/xewe-os-core) | `XeWeCore`, the Arduino library: serial console, CLI, NVS storage, helpers and the module system |
| [`xewe-os-modules`](https://github.com/xewe-labs/xewe-os-modules) | every module in one repo: WiFi, web interface, time, scheduler, buttons, pins |
| [`xewe-os-tools`](https://github.com/xewe-labs/xewe-os-tools) | `xewe`, one Python tool for setup, build, flash, serial and test |
| [`publish-arduino-library`](https://github.com/xewe-labs/publish-arduino-library) | checks and releases Arduino libraries; used to publish `XeWeCore` |

Boards: ESP32-C3, ESP32-C6 and ESP32-S3. Flashing is over serial only (no OTA).

There is no CI on purpose for now. Builds and tests run locally through `xewe-os-tools`, which
compiles for all three chips (`xewe build --all-chips`) and runs the tests with or without a board.

## Why it is shaped this way

The four repos split along one line: what you clone (`xewe-os`), what you build on (`xewe-os-core`),
what you pick (`xewe-os-modules`) and what builds it (`xewe-os-tools`). The template pins exact
versions of the other three in `xewe.lock`, so a project builds the same way next year. The
decisions behind this, the version policy and the migration from the older per-library and
per-module repositories are in
[xewe-os/ARCHITECTURE.md](https://github.com/xewe-labs/xewe-os/blob/main/ARCHITECTURE.md).

## Products

| Repository | |
|---|---|
| [`xewe-led-os`](https://github.com/xewe-labs/xewe-led-os) | the flagship: firmware for addressable LED devices. It moves onto xewe-os once the backbone has proven itself |
| [`xewe-led-os-homeassistant`](https://github.com/xewe-labs/xewe-led-os-homeassistant) | Home Assistant integration for a XeWe LED dock |
| [`xewe-os-frontend-builder`](https://github.com/xewe-labs/xewe-os-frontend-builder) | prototype a device web interface as a Flask app, export it into a sketch |

## Contributing

Rules and guidelines live in [`.github`](https://github.com/xewe-labs/.github). Start with its
[CONTRIBUTING.md](https://github.com/xewe-labs/.github/blob/main/CONTRIBUTING.md); a new module
starts with a [module proposal](https://github.com/xewe-labs/xewe-os-modules/issues/new?template=module_proposal.md)
and follows [CONTRACT.md](https://github.com/xewe-labs/xewe-os-modules/blob/main/CONTRACT.md).
See [`guidelines/repositories.md`](https://github.com/xewe-labs/.github/blob/main/guidelines/repositories.md)
for what belongs in which repository and how each is released.

Everything here is GPL-3.0-only unless a repository says otherwise.
