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

Boards: ESP32-C3, ESP32-C6 and ESP32-S3.

## XeWe OS

The firmware, the registry it installs from, and the tooling that builds it.

| Repository | |
|---|---|
| [`xewe-os`](https://github.com/xewe-labs/xewe-os) | the firmware: CLI, WiFi, network time, schedules, buttons, pin control |
| [`xewe-os-modules`](https://github.com/xewe-labs/xewe-os-modules) | registry of module repositories, read by `scripts/setup.sh` |
| [`xewe-os-build-toolchain`](https://github.com/xewe-labs/xewe-os-build-toolchain) | compile, flash, format and release scripts shared by every firmware |
| [`xewe-os-frontend-builder`](https://github.com/xewe-labs/xewe-os-frontend-builder) | prototype a device web interface as a Flask app, export it into a sketch |

### Example builds

| Repository | |
|---|---|
| [`xewe-os-laptop-cooling-pad`](https://github.com/xewe-labs/xewe-os-laptop-cooling-pad) | fan control for a laptop cooling pad *(archived)* |

## XeWe OS modules

One feature each, installed into a firmware by the registry.

| Repository | |
|---|---|
| [`xewe-os-module-wifi`](https://github.com/xewe-labs/xewe-os-module-wifi) | connects to a local WiFi network and keeps the connection alive |
| [`xewe-os-module-time`](https://github.com/xewe-labs/xewe-os-module-time) | NTP time sync and timezone handling |
| [`xewe-os-module-scheduler`](https://github.com/xewe-labs/xewe-os-module-scheduler) | runs stored commands on a weekly schedule |
| [`xewe-os-module-buttons`](https://github.com/xewe-labs/xewe-os-module-buttons) | binds commands to physical buttons, with software debouncing |
| [`xewe-os-module-pins`](https://github.com/xewe-labs/xewe-os-module-pins) | GPIO, ADC, PWM and I2C access from the command line |
| [`xewe-os-module-web-interface`](https://github.com/xewe-labs/xewe-os-module-web-interface) | HTTP page and command endpoint for other devices on the network |

## XeWe libraries

Arduino libraries (`xewe-library-*`) that also work on their own.

| Repository | |
|---|---|
| [`xewe-library-os`](https://github.com/xewe-labs/xewe-library-os) | XeWeOS: module lifecycle, controller, `$system` |
| [`xewe-library-cli`](https://github.com/xewe-labs/xewe-library-cli) | serial command line, `$<group> <command> [args...]` |
| [`xewe-library-serial`](https://github.com/xewe-labs/xewe-library-serial) | non-blocking serial console |
| [`xewe-library-nvs`](https://github.com/xewe-labs/xewe-library-nvs) | typed key-value storage on the ESP32 NVS partition |
| [`xewe-library-utils`](https://github.com/xewe-labs/xewe-library-utils) | header-only helpers used across the libraries, no dependencies |

## XeWe LED OS

Firmware for addressable LED devices, and what connects to it.

| Repository | |
|---|---|
| [`xewe-led-os`](https://github.com/xewe-labs/xewe-led-os) | the firmware: ultimate OS for addressable LED with ESP32 |
| [`xewe-led-os-homeassistant`](https://github.com/xewe-labs/xewe-led-os-homeassistant) | Home Assistant integration for a XeWe LED dock |
| [`xewe-led-os-frontend`](https://github.com/xewe-labs/xewe-led-os-frontend) | web interface for the firmware *(archived)* |
| [`xewe-led-os-draft`](https://github.com/xewe-labs/xewe-led-os-draft) | the first draft of the firmware *(archived)* |
| [`xewe-led-web-draft`](https://github.com/xewe-labs/xewe-led-web-draft) | simple web interface to control LED lights *(archived)* |

Forked Arduino libraries the LED firmware was built on, kept for reference and archived:
[espalexa](https://github.com/xewe-labs/xewe-led-library-espalexa),
[homespan](https://github.com/xewe-labs/xewe-led-library-homespan),
[websockets](https://github.com/xewe-labs/xewe-led-library-websockets).

### Example builds

One device each, from before the module layout. All archived.

| Repository | |
|---|---|
| [`xewe-led-bed`](https://github.com/xewe-labs/xewe-led-bed) | bed lights controlled by Amazon Alexa |
| [`xewe-led-ceiling`](https://github.com/xewe-labs/xewe-led-ceiling) | ceiling lights |
| [`xewe-led-desk`](https://github.com/xewe-labs/xewe-led-desk) | desk lights |
| [`xewe-led-bathroom`](https://github.com/xewe-labs/xewe-led-bathroom) | bathroom lights |
| [`xewe-led-wardrobe`](https://github.com/xewe-labs/xewe-led-wardrobe) | wardrobe lights |
| [`xewe-led-retro-christmas-lights`](https://github.com/xewe-labs/xewe-led-retro-christmas-lights) | sets an addressable strip in the Christmas mood |
| [`xewe-led-hookah-os`](https://github.com/xewe-labs/xewe-led-hookah-os) | lights that illuminate a hookah's flask |
| [`xewe-led-poster-os`](https://github.com/xewe-labs/xewe-led-poster-os) | competition poster with live LED lights driven by Arduino |
| [`xewe-led-buggy`](https://github.com/xewe-labs/xewe-led-buggy) | lights for a buggy |
| [`xewe-led-suv-bot`](https://github.com/xewe-labs/xewe-led-suv-bot) | lights for an SUV bot |
| [`xewe-led-quantum-controller`](https://github.com/xewe-labs/xewe-led-quantum-controller) | standalone LED controller firmware |
| [`xewe-led-quantum-controller-app`](https://github.com/xewe-labs/xewe-led-quantum-controller-app) | desktop app for that controller |

## Contributing

Rules and guidelines live in [`.github`](https://github.com/xewe-labs/.github). Start with its
[CONTRIBUTING.md](https://github.com/xewe-labs/.github/blob/main/CONTRIBUTING.md), and see
[`guidelines/repositories.md`](https://github.com/xewe-labs/.github/blob/main/guidelines/repositories.md)
for what each repository family is named and how it is released.

Everything here is GPL-3.0-only unless a repository says otherwise.
