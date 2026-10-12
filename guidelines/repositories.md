# Repositories

XeWe OS is four repositories plus one kept tool, and this `.github` repository holds the
organization's rules. Each has one job; code goes where its job is.
Why the split is drawn this way is in
[`xewe-os/doc/architecture.md`](https://github.com/xewe-labs/xewe-os/blob/main/doc/architecture.md).

| Repository | Holds | Belongs here | Released as |
|---|---|---|---|
| `xewe-os-core` | the `XeWeCore` Arduino library | reusable code every firmware needs: utils, serial console, CLI, NVS/FlexData, the `XeWeOs` facade and `xewe::Module` | semver tag `X.Y.Z` (no `v`) by `scripts/release.sh`; the Arduino Library Manager picks the tag up |
| `xewe-os-modules` | every module, under `modules/<slug>/` | code figured out once: one feature (WiFi, a schedule, a sensor) a firmware can select | per-module tags `<module>/X.Y.Z` and the set tag `X.Y.Z`, by `scripts/release.sh`; each module declares `requires_core` |
| `xewe-os-tools` | the Python package `xewe` | everything that builds, flashes, talks to or tests a board: setup, build, flash, serial, provision, test, boards, modules, manifest, format, brand-lint, release; the reusable CI workflows | tag `X.Y.Z` by `scripts/release.sh` |
| `xewe-os` | the firmware template (a GitHub template repository) | the thing users clone: sketch, `Config.h`, `xewe.toml`, two example modules, `setup.sh`, docs | its own version in `xewe.toml [project]`, tag `X.Y.Z` by `scripts/release.sh`; CI builds the firmware, publishes a GitHub Release and commits the binaries to `main` |
| `publish-arduino-library` | a generic Arduino-library publishing tool | checking Arduino libraries; XeWeCore releases with its own `scripts/release.sh` | not tagged |
| `.github` | organization profile, guidelines, issue and pull request templates | rules that span repositories | not released |

Rules of thumb:

* **Would a second firmware need it?** Then it is core (if every firmware needs it) or a module
  (if some do), never template code.
* **Does it run on the host, not the chip?** It is tools. The modules repo and the template carry
  no toolchain of their own; modules are built and tested through an `xewe-os` checkout.
* **A project built from the template** keeps its own code in the sketch and `Config.h`. A feature
  that turns out reusable moves into the modules repo as a pull request.

CI runs on GitHub Actions through reusable workflows in `xewe-os-tools/.github/workflows/`
(`xewe-build.yml`, `xewe-release.yml`, `xewe-core-tests.yml`, `xewe-modules-tests.yml`); each
repository holds only a short caller in its own `.github/workflows/` that names a tools ref (the
template `@3.0.1`, the same as its `[tools] ref`; modules `@3.0.0`; core `@main`), and
[`xewe-os-tools/doc/ci.md`](https://github.com/xewe-labs/xewe-os-tools/blob/main/doc/ci.md)
documents them. CI runs the same commands as a laptop (`./setup.sh`, `xewe build`, `xewe test`
without a board, `xewe format --check` and `xewe brand-lint`, both blocking; core also compiles
its examples for c3, c6 and s3), and `xewe release` on an `X.Y.Z` tag of the template, which
publishes a GitHub Release and commits the release folder to `main`. Board tests stay local, with
a board attached.

## Naming

Repository names are lowercase with dashes. Everything that is part of XeWe OS is prefixed
`xewe-os-*` (the template itself is `xewe-os`). Modules are folders, not repositories:
`modules/<slug>/` in `xewe-os-modules`. A firmware built from the template is named for the
device (for example `xewe-led-os`). Older `xewe-led-*` repositories predate this and are archived
rather than renamed.

## Committed vs generated

Nothing generated is committed (the two exceptions are below), and there are no submodules. The template's dependencies come from
the refs in `xewe.toml`, fetched by `./setup.sh`.

| Repository | Committed | Generated, ignored |
|---|---|---|
| `xewe-os` (and every project cloned from it) | `xewe-os.ino`, `Config.h`, `xewe.toml`, `src/<YourModule>/`, `setup.sh`, `tests/`, `doc/`, `.agents/`, `scripts/release.sh`, `releases/<version>/`, `.gitattributes`, `static/` (web assets, and `static/firmware/releases/` written by the CI release commit, `.bin` in Git LFS) | `build/` (tools venv, XeWeCore, libraries, the generated `build/modules/` with `modules.lock`, `builds/<chip>/out/`), `src/Modules.h`, `run.sh` |
| `xewe-os-core` | library sources, examples, `doc/`, `tests/`, `library.properties`, `library.json`, `scripts/release.sh`, `releases/<version>/` | build output of examples and host tests |
| `xewe-os-modules` | `modules/<slug>/`, `tools/validate.py`, `libraries.toml`, `doc/contract.md`, `doc/modules.md`, `scripts/release.sh`, `releases/<version>/` | `__pycache__/`, `.pytest_cache/`, `build/`, `.venv/` |
| `xewe-os-tools` | the package, its tests, `doc/`, the reusable workflows, `scripts/release.sh`, `releases/<version>/` | `.venv/`, `build/`, `*.egg-info/`, caches |

arduino-cli, the esp32 core and the modules checkouts live outside every project, in the shared
`~/.xewe-os/build-tools/`.

One generated file is committed on purpose: the modules' `doc/modules.md` (written by
`tools/validate.py --write-index`, checked by the validator). Core's `library.json` is committed
for the registries and carries the same version as `library.properties`; `scripts/release.sh
--prepare` sets both.

**Firmware releases are committed by CI, never by hand.** CI builds them from a version tag with
`xewe release`, publishes them as a GitHub Release, and then one bot commit adds
`static/firmware/releases/<version>/` to the template's `main`, with the `.bin` files in Git LFS
([`git-and-releases.md`](git-and-releases.md#release-channels)).

## Versions

| Repository | Version lives in | Tag | Notes |
|---|---|---|---|
| `xewe-os-core` | `library.properties` `version=` (mirrored into `library.json` and `XEWE_CORE_VERSION*`) | `X.Y.Z` | semver, picked up by the Arduino Library Manager; 3.0.0 |
| `xewe-os-modules` | each module's `module.properties` `version=` | `<module>/X.Y.Z` per module, `X.Y.Z` for the set | each module declares `requires_core`, comma form (`>=3.0.0,<4.0.0`); projects pin the set tag; 3.0.0 |
| `xewe-os-tools` | `pyproject.toml` (and the other locations `scripts/release.sh` lists) | `X.Y.Z` | 3.0.0, 3.0.1 |
| `xewe-os` | `xewe.toml` `[project] version` | `X.Y.Z` | independent of the others; 3.0.0 |

Tags carry no `v` from 3.0.0 on; older `v` tags keep their names.

The template's `xewe.toml` names one ref each for core, modules and tools, plus third-party
libraries (`[libraries]`, ArduinoJson). The template pins core `3.0.0`, modules `3.0.0`, tools
`3.0.1` and ArduinoJson `v7.4.3`. `xewe manifest update` moves the refs to the newest tags, and
`./setup.sh --latest` tries the newest tags without editing `xewe.toml`. The full release flow is
in [`git-and-releases.md`](git-and-releases.md).

## Every repository has

* `README.md`, see [`documentation.md`](documentation.md)
* agent files: `.agents/` (the WAX workspace: `AGENTS.md` for how to work in the repository,
  `RULES.md` whose project rules summarise these guidelines). See [`documentation.md`](documentation.md#agent-files)
* `LICENSE.txt`: GPL-3.0-only, unless the repository states otherwise
* `.gitignore`: at minimum `.DS_Store`, `.idea/`, `.vscode/`, and whatever that repository generates
* `main` as the default branch

Source files start with the header in [`license-header.txt`](license-header.txt).

## Repository layouts

**Core** follows the Arduino library layout at the repo root, with one top-level header,
`src/XeWeCore.h`, and everything else in `src/XeWeCore/`. The rules the publishing tool enforces
are in
[`publish-arduino-library/docs/library-rules.md`](https://github.com/xewe-labs/publish-arduino-library/blob/main/docs/library-rules.md).

**Modules** see [`modules.md`](modules.md) and `xewe-os-modules/doc/contract.md`.

**Tools** is a self-contained Python package (`pyproject.toml`, `src/`, `tests/`, `doc/`,
`scripts/`). It never stores project data: it is installed into a project's `build/tools/.venv`,
and everything specific to the project stays in the project. `setup.sh` belongs to the template;
`xewe setup` writes each project's `run.sh`.

**Template** has the sketch and `Config.h` at the root, `setup.sh` as a thin wrapper around
`xewe` (and the `run.sh` it generates), `xewe.toml`, project-local modules in `src/<Folder>/`, the generated `src/Modules.h`,
`build/` for everything installed and generated, and `static/` for web assets and media.

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
new repository changes how the ecosystem works, update these guidelines and
`xewe-os/doc/architecture.md` in the same breath.
