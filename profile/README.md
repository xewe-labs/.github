# xewe-os ecosystem

Firmware always contains reusable bits and pieces. xewe-os builds on this fact with a philosophy of
a highly modular framework that is ready for agent-first development.

What it has: [`xewe-os-core`](https://github.com/xewe-labs/xewe-os-core) contains the core
functions, such as serial port interaction, the CLI engine, memory storage and others. Then there
is a module repo, [`xewe-os-modules`](https://github.com/xewe-labs/xewe-os-modules), that has
validated and tested modules. For example, need buttons? Install buttons. Need LED control or a
fan? Install the module. Then, to build and test it, we have
[`xewe-os-tools`](https://github.com/xewe-labs/xewe-os-tools). It also contains some dev tools for
rapid prototyping, but its primary purpose is to compile and test the code. It all comes together
with [`xewe-os`](https://github.com/xewe-labs/xewe-os), a template that pulls the core, the modules
of your choice and the build tools together. With only two commands, `setup.sh` and `run.sh`, you
can go from an idea to flashed firmware.

<picture><source media="(prefers-color-scheme: dark)" srcset="ecosystem-dark.svg"><img alt="xewe-os ecosystem: core, modules and custom modules flow into xewe-os from the top, tools enter from the side; xewe-led-os and xewe-chiller-os are built on it" src="ecosystem-light.svg" width="960"></picture>

## Releases and CI/CD

Every release has a generated `release_card.md`: release notes, metadata, validator output, one
row per test with its result (hardware runs are marked apart from compile-only ones), plus the
logs. v3 marks the ecosystem unification.

* `xewe-os-tools`: no binaries, just the release tag.
* `xewe-os-core`: no binaries, just the release tag; its release card lists the unit and on-board
  tests, one test per row with its result.
* `xewe-os-modules`: a release per module (a `release_card.md` per module) and a set tag; still no
  binaries.
* `xewe-os`: release tags for the harness, plus precompiled binaries published as GitHub Release
  assets (never committed to git). Flashable from [maxdokukin.com](https://maxdokukin.com), and
  later from GitHub Pages.

From this ecosystem emerge xewe-led-os and xewe-chiller-os.

## xewe-led-os

[`xewe-led-os`](https://github.com/xewe-labs/xewe-led-os) is the LED strip firmware for ESP32-C3,
-C6 and -S3: every FastLED chipset selectable at runtime, 7 effects, a web page, HomeKit, Alexa,
Home Assistant, schedules and buttons. It is built on xewe-os 3.0 with the `led` and `led-web`
modules plus its own project-local modules.

Status: 3.0 is on branch `v3`, hardware verification in progress; the 2.x history is kept under
the `v2.3.0` tag.

## xewe-chiller-os

The laptop cooling pad firmware (fan, temperature and LED), currently
[`xewe-os-laptop-cooling-pad`](https://github.com/xewe-labs/xewe-os-laptop-cooling-pad). It will
be rebranded xewe-chiller-os and migrated to xewe-os 3.0 the same way.

## Repositories

### Ecosystem

* [`xewe-os`](https://github.com/xewe-labs/xewe-os): lightweight OS for ESP32 and Arduino; the template
* [`xewe-os-core`](https://github.com/xewe-labs/xewe-os-core): the XeWeCore library: serial, CLI engine, storage
* [`xewe-os-modules`](https://github.com/xewe-labs/xewe-os-modules): registry of xewe-os modules read by `scripts/setup.sh`
* [`xewe-os-tools`](https://github.com/xewe-labs/xewe-os-tools): the `xewe` CLI: setup, build, flash, test, release
* [`.github`](https://github.com/xewe-labs/.github): this profile, CONTRIBUTING.md and the shared templates

### xewe-led-os

* [`xewe-led-os`](https://github.com/xewe-labs/xewe-led-os): LED strip firmware for ESP32-C3, -C6 and -S3
* [`xewe-led-os-homeassistant`](https://github.com/xewe-labs/xewe-led-os-homeassistant): Home Assistant integration for XeWe LED OS devices
* [`library-homespan`](https://github.com/xewe-labs/library-homespan): fork: HomeKit library for the Arduino-ESP32 (archived)
* [`library-espalexa`](https://github.com/xewe-labs/library-espalexa): fork: Alexa voice control for ESP8266/ESP32 (archived)
* [`library-websockets`](https://github.com/xewe-labs/library-websockets): fork: arduinoWebSockets (archived)
* [`led-reliable-dock`](https://github.com/xewe-labs/led-reliable-dock): reliable LED dock: build photos and design PDF
* [`led-os-frontend`](https://github.com/xewe-labs/led-os-frontend): Flask prototype of the XeWe LED OS scheduler UI, exported to firmware (archived)
* [`led-web-ui`](https://github.com/xewe-labs/led-web-ui): Flask prototype of a multi-device LED control page, second iteration (archived)
* [`led-web-draft`](https://github.com/xewe-labs/led-web-draft): simple web interface to control LED lights (archived)
* [`led-os-draft`](https://github.com/xewe-labs/led-os-draft): history, redirects to `xewe-led-os`
* [`xewe-led-os-orphan`](https://github.com/xewe-labs/xewe-led-os-orphan): superseded 2026 attempt
* [`xewe-led-quantum-controller-app`](https://github.com/xewe-labs/xewe-led-quantum-controller-app): merged into `led-quantum-controller` under `app/` (archived)

### xewe-chiller-os

* [`xewe-os-laptop-cooling-pad`](https://github.com/xewe-labs/xewe-os-laptop-cooling-pad): laptop cooling pad firmware (fan, temperature, LED); to become xewe-chiller-os

### Earlier LED projects (archived)

* [`led-bathroom`](https://github.com/xewe-labs/led-bathroom): Alexa and HomeKit lights around a bathroom mirror (archived)
* [`led-bed`](https://github.com/xewe-labs/led-bed): LED lights controlled by Amazon Alexa (archived)
* [`led-buggy`](https://github.com/xewe-labs/led-buggy): animated RGB strips and a glowing logo for a dune buggy (archived)
* [`led-ceiling`](https://github.com/xewe-labs/led-ceiling): voice-controlled ceiling lights on an ESP8266 (archived)
* [`led-desk`](https://github.com/xewe-labs/led-desk): ESP8266 desk and ceiling lights for HomeKit and Alexa (archived)
* [`led-hookah-os`](https://github.com/xewe-labs/led-hookah-os): LED lights that illuminate a hookah's flask (archived)
* [`led-poster-os`](https://github.com/xewe-labs/led-poster-os): competition poster with live LED lights controlled by Arduino (archived)
* [`led-quantum-controller`](https://github.com/xewe-labs/led-quantum-controller): Arduino Nano firmware and Windows app for four PC lighting channels (archived)
* [`led-retro-christmas-lights`](https://github.com/xewe-labs/led-retro-christmas-lights): an addressable LED strip in the Christmas mood (archived)
* [`led-suv-bot`](https://github.com/xewe-labs/led-suv-bot): solar-charged cocktail stand with glass-triggered LED animations (archived)
* [`led-wardrobe`](https://github.com/xewe-labs/led-wardrobe): smart wardrobe lighting on an ESP8266 with HomeKit and Alexa (archived)

Why it is shaped this way: [xewe-os/doc/architecture.md](https://github.com/xewe-labs/xewe-os/blob/main/doc/architecture.md).

Arduino IDE only? Install `XeWeCore` from the Library Manager and start with the
[`01_Hello`](https://github.com/xewe-labs/xewe-os-core/tree/main/examples/01_Hello) example.

Contributing: [CONTRIBUTING.md](https://github.com/xewe-labs/.github/blob/main/CONTRIBUTING.md);
a new module starts with a [module proposal](https://github.com/xewe-labs/xewe-os-modules/issues/new?template=module_proposal.md).

Everything here is GPL-3.0-only unless a repository says otherwise.
