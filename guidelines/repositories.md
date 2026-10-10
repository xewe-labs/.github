# Repositories

XeWe OS is four repositories plus one kept tool, and this `.github` repository holds the
organization's rules. Each has one job; code goes where its job is.
Why the split is drawn this way is in
[`xewe-os/ARCHITECTURE.md`](https://github.com/xewe-labs/xewe-os/blob/main/ARCHITECTURE.md).

| Repository | Holds | Belongs here | Released as |
|---|---|---|---|
| `xewe-os-core` | the `XeWeCore` Arduino library | reusable code every firmware needs: utils, serial console, CLI, NVS/FlexData, the `XeWeOs` facade and `xewe::Module` | semver tag `X.Y.Z` (no `v`), Arduino Library Manager, PlatformIO |
| `xewe-os-modules` | every module, under `modules/<slug>/` | code figured out once: one feature (WiFi, a schedule, a sensor) a firmware can select | repo tag `vX.Y.Z`; each module declares `requires_core` |
| `xewe-os-tools` | the Python package `xewe` | everything that builds, flashes, talks to or tests a board: setup, build, flash, serial, provision, test, boards, modules, manifest, release | tag `vX.Y.Z` |
| `xewe-os` | the firmware template (a GitHub template repository) | the thing users clone: sketch, `Config.h`, `xewe.toml`, two example modules, `setup.sh`, `run.sh`, docs | its own version in `xewe.toml [project]`; firmware releases under `static/firmware/releases/` |
| `publish-arduino-library` | a generic Arduino-library publishing tool | checking and releasing `XeWeCore` (and any other Arduino library) | tag `vX.Y.Z` |
| `.github` | organization profile, guidelines, issue and pull request templates | rules that span repositories | not released |

Rules of thumb:

* **Would a second firmware need it?** Then it is core (if every firmware needs it) or a module
  (if some do), never template code.
* **Does it run on the host, not the chip?** It is tools. The modules repo and the template carry
  no toolchain of their own; modules are built and tested through an `xewe-os` checkout.
* **A project built from the template** keeps its own code in the sketch and `Config.h`. A feature
  that turns out reusable moves into the modules repo as a pull request.

There is no CI for now, by decision. Builds and tests are local: `xewe build --all-chips` and
`xewe test` from `xewe-os-tools` are the gate, with or without a board.

## Naming

Repository names are lowercase with dashes. Everything that is part of XeWe OS is prefixed
`xewe-os-*` (the template itself is `xewe-os`). Modules are folders, not repositories:
`modules/<slug>/` in `xewe-os-modules`. A firmware built from the template is named for the
device (for example `xewe-led-os`). Older `xewe-led-*` repositories predate this and are archived
rather than renamed.

## Committed vs generated

Nothing generated is committed, and there are no submodules. The template's dependencies come from
the refs in `xewe.toml`, fetched by `./setup.sh`.

| Repository | Committed | Generated, ignored |
|---|---|---|
| `xewe-os` (and every project cloned from it) | `xewe-os.ino`, `Config.h`, `xewe.toml`, `src/<YourModule>/`, `setup.sh`, `run.sh`, docs, `.agents/`, `static/firmware/releases/` | `build/` (tools venv, XeWeCore, libraries, the generated `build/modules/` with `modules.lock`, `builds/<chip>/out/`), `src/Modules.h` |
| `xewe-os-core` | library sources, examples, `doc/`, `tests/`, `library.properties`, `library.json` | build output of examples and host tests |
| `xewe-os-modules` | `modules/<slug>/`, `tools/validate.py`, `libraries.toml`, `MODULES.md` | `__pycache__/`, `.pytest_cache/`, `build/`, `.venv/` |
| `xewe-os-tools` | the package, its tests, `scripts/` | `.venv/`, `build/`, `*.egg-info/`, caches |

arduino-cli, the esp32 core and the modules checkouts live outside every project, in the shared
`~/.xewe-os/build-tools/`.

Two generated files are committed on purpose: `library.json` (registries read it from the
repository; regenerate it with `publish.py manifest`, never hand-edit) and `MODULES.md` (written by
`tools/validate.py --write-index`, checked by the validator).

**Firmware releases are committed** under `static/firmware/releases/<version>/` (binaries,
manifests, notes, `firmware-<version>.tar.gz`), written by `xewe release`. The web flasher reads
them from there.

## Versions

| Repository | Version lives in | Tag | Notes |
|---|---|---|---|
| `xewe-os-core` | `library.properties` `version=` (mirrored into `library.json`) | `X.Y.Z`, un-prefixed | semver, published to the Arduino Library Manager |
| `xewe-os-modules` | the repo tag; each module's `module.properties` `version=` is informational | `vX.Y.Z` | each module declares `requires_core`, comma form (`>=2.1.0,<3.0.0`) |
| `xewe-os-tools` | `pyproject.toml` | `vX.Y.Z` | |
| `xewe-os` | `xewe.toml` `[project] version` | `vX.Y.Z`, optional | independent of the others |

The template's `xewe.toml` names one ref each for core, modules and tools, plus third-party
libraries (`[libraries]`, ArduinoJson). Until the ecosystem's `v3.0.0` tag set the refs are
`latest` (the newest commit of each default branch); after it, `xewe manifest update` moves them to
the newest tags, and `./setup.sh --latest` tries the newest tags without editing `xewe.toml`. The
full release flow is in [`git-and-releases.md`](git-and-releases.md).

## Every repository has

* `README.md`, see [`documentation.md`](documentation.md)
* agent files: `.agents/` (the WAX workspace: `AGENTS.md` for how to work in the repository,
  `RULES.md` whose project rules summarise these guidelines); the tools keep a root `AGENTS.md`.
  See [`documentation.md`](documentation.md#agent-files)
* `LICENSE.txt`: GPL-3.0-only, unless the repository states otherwise
* `.gitignore`: at minimum `.DS_Store`, `.idea/`, `.vscode/`, and whatever that repository generates
* `main` as the default branch

Source files start with the header in [`license-header.txt`](license-header.txt).

## Repository layouts

**Core** follows the Arduino library layout at the repo root, with one top-level header,
`src/XeWeCore.h`, and everything else in `src/XeWeCore/`. The rules the publishing tool enforces
are in
[`publish-arduino-library/docs/library-rules.md`](https://github.com/xewe-labs/publish-arduino-library/blob/main/docs/library-rules.md).

**Modules** see [`modules.md`](modules.md) and `xewe-os-modules/CONTRACT.md`.

**Tools** is a self-contained Python package (`pyproject.toml`, `src/`, `tests/`, `scripts/`). It
never stores project data: it is installed into a project's `build/tools/.venv`, and everything
specific to the project stays in the project. Its `scripts/setup.sh` and `scripts/run.sh` are the
reference copies of the template's scripts.

**Template** has the sketch and `Config.h` at the root, `setup.sh` and `run.sh` as thin wrappers
around `xewe`, `xewe.toml`, project-local modules in `src/<Folder>/`, the generated `src/Modules.h`,
`build/` for everything installed and generated, and `static/` for released binaries and media.

## Repository settings

* **Public by default.** Keep a repository private only while it has no usable version, or when it
  holds credentials-adjacent material.
* **Description filled in**, one line, so the organization page reads well.
* `xewe-os` is marked as a **template repository**, and also works as a plain `git clone`.
* **Archive rather than delete** a repository that is no longer developed.

## Adding a repository

The four-repo split is deliberate; a new `xewe-os-*` repository needs a reason none of the four can
hold. A new module is not a repository: open a module proposal and a pull request to
`xewe-os-modules`. A new firmware product is a project created from the `xewe-os` template. If a
new repository changes how the ecosystem works, update these guidelines and `ARCHITECTURE.md` in
the same breath.
