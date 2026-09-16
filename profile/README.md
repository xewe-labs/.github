# XeWe Labs

Firmware, libraries and tooling for ESP32 devices you control from a command line — over serial,
over HTTP, or from another device on the network.

## Start here

| | |
|---|---|
| [**xewe-os**](https://github.com/xewe-labs/xewe-os) | ready-to-flash ESP32 firmware: CLI, WiFi, network time, schedules, buttons, pin control |
| [**xewe-led-os**](https://github.com/xewe-labs/xewe-led-os) | firmware for addressable LED devices |
| [**xewe-library-os**](https://github.com/xewe-labs/xewe-library-os) | the XeWeOS framework: module lifecycle, controller, `$system` |

Firmware is assembled from modules. Each module is its own repository, listed in the
[module registry](https://github.com/xewe-labs/xewe-os-modules); anyone can add one with a pull
request.

## The pieces

* **Libraries** (`xewe-library-*`) — Arduino libraries that also work on their own:
  [utils](https://github.com/xewe-labs/xewe-library-utils),
  [serial](https://github.com/xewe-labs/xewe-library-serial),
  [nvs](https://github.com/xewe-labs/xewe-library-nvs),
  [cli](https://github.com/xewe-labs/xewe-library-cli),
  [os](https://github.com/xewe-labs/xewe-library-os)
* **Modules** (`xewe-os-module-*`) — one feature each: WiFi, time, scheduler, buttons, pins, web interface
* **Tooling** — [build toolchain](https://github.com/xewe-labs/xewe-os-build-toolchain) for
  compiling, flashing, formatting and releasing

Boards: ESP32-C3, ESP32-C6 and ESP32-S3.

## Contributing

Rules and guidelines live in [`.github`](https://github.com/xewe-labs/.github). Start with its
[CONTRIBUTING.md](https://github.com/xewe-labs/.github/blob/main/CONTRIBUTING.md).

Everything here is GPL-3.0-only unless a repository says otherwise.
