# XeWe Labs

XeWe is a small ecosystem for ESP32-C3, -C6 and -S3 firmware that you control from a command
line, over serial or over the network. You clone one template, pick modules, and build with one
local tool. A module is a piece of code someone (often a coding agent) figured out once, so nobody
has to figure it out again. Humans and agents use the same commands, and everything still works
with no board attached: builds compile and tests report "compiled, not run".

## Start here

Pick the level that fits:

| Level | You use | Start with |
|---|---|---|
| 1 | the Arduino IDE: Library Manager → `XeWeCore` | the console, NVS and prompts: example [`01_Hello`](https://github.com/xewe-labs/xewe-os-core/tree/main/examples/01_Hello) |
| 2 | the same, plus one module of your own in the sketch folder | example [`02_MyModule`](https://github.com/xewe-labs/xewe-os-core/tree/main/examples/02_MyModule) |
| 3 | the [**xewe-os**](https://github.com/xewe-labs/xewe-os) template and the `xewe` tools | clone it, `./setup.sh`, `./run.sh`: ready-made modules, multi-chip builds, board tests, releases |

The template's [README](https://github.com/xewe-labs/xewe-os#readme) covers the first five minutes;
[doc/architecture.md](https://github.com/xewe-labs/xewe-os/blob/main/doc/architecture.md) explains
why it is shaped this way.

## XeWe OS

| Repository | Release | |
|---|---|---|
| [`xewe-os`](https://github.com/xewe-labs/xewe-os) | [`3.0.0`](https://github.com/xewe-labs/xewe-os/releases/tag/3.0.0) | the firmware template you clone (or "Use this template"); holds only your code and `xewe.toml` |
| [`xewe-os-core`](https://github.com/xewe-labs/xewe-os-core) | [`3.0.0`](https://github.com/xewe-labs/xewe-os-core/releases/tag/3.0.0) | `XeWeCore`, the Arduino library: serial console, CLI, NVS storage, helpers and the module system |
| [`xewe-os-modules`](https://github.com/xewe-labs/xewe-os-modules) | [`3.0.0`](https://github.com/xewe-labs/xewe-os-modules/releases/tag/3.0.0) | every module in one repo: Wi-Fi, web interface, time, scheduler, buttons, pins |
| [`xewe-os-tools`](https://github.com/xewe-labs/xewe-os-tools) | [`3.0.1`](https://github.com/xewe-labs/xewe-os-tools/releases/tag/3.0.1) | `xewe`, one Python tool for setup, build, flash, serial, provisioning, test, format and release, plus the CI workflows and `xewe webui` (a device web UI prototyped in Flask, exported to firmware) |

Boards: ESP32-C3, ESP32-C6 and ESP32-S3. Flashing is over serial only (no OTA).

Builds and tests run the same way on a laptop and in CI: `xewe-os-tools` compiles for all three
chips (`xewe build --all-chips`) and runs the tests with or without a board, and its reusable
GitHub Actions workflows run those commands on every push; the format check (`xewe format`) and
`xewe brand-lint` are blocking jobs. A template release tag builds c3, c6 and s3 and publishes the
binaries as a GitHub Release.

## Why it is shaped this way

The four repos split along one line: what you clone (`xewe-os`), what you build on (`xewe-os-core`),
what you pick (`xewe-os-modules`) and what builds it (`xewe-os-tools`). A project's `xewe.toml`
names one ref of each of the other three. The repositories were tagged `3.0.0` together, and the
template pins those tags (tools `3.0.1`), so a project builds the same way next year;
`xewe manifest update` moves it to newer tags. The decisions behind this and the version policy
are in
[xewe-os/doc/architecture.md](https://github.com/xewe-labs/xewe-os/blob/main/doc/architecture.md).

## Products

| Repository | |
|---|---|
| [`xewe-led-os`](https://github.com/xewe-labs/xewe-led-os) | the flagship: firmware for addressable LED devices. It moves onto xewe-os once the backbone has proven itself |
| [`xewe-led-os-homeassistant`](https://github.com/xewe-labs/xewe-led-os-homeassistant) | Home Assistant integration for a XeWe LED dock |

## Contributing

Rules and guidelines live in [`.github`](https://github.com/xewe-labs/.github). Start with its
[CONTRIBUTING.md](https://github.com/xewe-labs/.github/blob/main/CONTRIBUTING.md); a new module
starts with a [module proposal](https://github.com/xewe-labs/xewe-os-modules/issues/new?template=module_proposal.md)
and follows [doc/contract.md](https://github.com/xewe-labs/xewe-os-modules/blob/main/doc/contract.md).
See [`guidelines/repositories.md`](https://github.com/xewe-labs/.github/blob/main/guidelines/repositories.md)
for what belongs in which repository and how each is released.

Everything here is GPL-3.0-only unless a repository says otherwise.
