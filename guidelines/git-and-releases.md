# Git and releases

## Branches

`main` is always buildable. Work on a branch named for the change (`fix-upload-timeout`,
`module-relay`), and merge through a pull request. Direct commits to `main` are for the
maintainer's own small changes only.

## Commits

One topic per commit. Subject in the imperative, lowercase, no trailing period, under 72
characters:

```
add scheduler module
fix wifi reconnect after deep sleep
remove esptool installation prompt
```

Add a body when the reason is not obvious from the diff: what was wrong, what changed, what it
affects. Two cases that always need one:

* **A change that breaks projects until they re-run setup** — a changed `xewe.lock` key, a moved
  file, a changed generated layout. Say so in the message.
* **A change made mostly by an agent.** Keep the `Co-Authored-By` trailer that records it.

Never commit generated or installed files: `build/` (venv, toolchain, fetched core, libraries and
modules, `out/`), `src/modules/` (with `Modules.h` and `modules.lock`), `.venv/`, caches. Two
exceptions are generated but committed: `library.json` (registries read it from the repository)
and `MODULES.md` in `xewe-os-modules`. Firmware releases under `static/firmware/releases/` are
committed on purpose. The full table is in [`repositories.md`](repositories.md).

## Versions

| | Where the version lives | Format | Tag |
|---|---|---|---|
| `xewe-os-core` (XeWeCore) | `library.properties` → `version=`, mirrored into `library.json` | `X.Y.Z` | `X.Y.Z`, un-prefixed |
| `xewe-os-modules` | the repo tag; each module's `module.properties` → `version=` is informational | `X.Y.Z` | `vX.Y.Z` |
| `xewe-os-tools` | `pyproject.toml` → `version` | `X.Y.Z` | `vX.Y.Z` |
| `xewe-os` firmware | `xewe.lock` → `[project] version` | `X.Y.Z` | `vX.Y.Z` |

Semantic versioning, read from the consumer's side: MAJOR when existing firmware, sketches or
scripts must change, MINOR for new capability that older callers ignore, PATCH for fixes. Each
module declares the core range it builds against, `requires_core=>=2.0.0,<3.0.0`. The template's
version is independent of the others; its `xewe.lock` pins one ref each for core, modules and
tools.

**Builds never change a version.** The firmware version is `[project] version` in `xewe.lock`, and
only `xewe release --version X.Y.Z` writes it. There is no build counter and no `version_state`.

XeWeCore's version is bumped by the publishing tool, not by hand.

## Tags and releases

A tag is permanent: registries reject a reused version, and projects pin to what you published in
`xewe.lock`. Never move or delete a published tag; release a new patch version instead. Agents
never tag, push or publish; every release step below is run by a human.

| Repository | How it is released |
|---|---|
| `xewe-os-core` | through [`publish-arduino-library`](https://github.com/xewe-labs/publish-arduino-library): `publish.py check xewe-os-core` (layout, metadata, lint, every example compiles), then `publish.py release xewe-os-core` (prints the plan), then `--execute` (or `scripts/publish.sh`), which bumps the version, tags `X.Y.Z` and creates the GitHub release. Read the plan before executing. Listing in the Arduino Library Manager is a separate one-time step (`publish.py register`, or a registry change request when the URL or name of an existing entry changes) |
| `xewe-os-modules` | `tools/validate.py` exits 0 and the module gate passes in a harness, then an annotated tag `vX.Y.Z` on `main`. Projects move to it with `xewe lock update` |
| `xewe-os-tools` | its pytest suite passes, then an annotated tag `vX.Y.Z`. The template's `[tools] ref` pins it |
| `xewe-os` (and every firmware built from it) | `xewe release --version X.Y.Z [--notes FILE]` builds the release matrix (`release_matrix.csv`, default c3/c6/s3), writes binaries, manifests and notes into `static/firmware/releases/<version>/` plus `firmware-<version>.tar.gz`, sets `[project] version`, and prints the `git`/`gh` commands (commit, tag `v<version>`, push, GitHub release) for a human to run |

Firmware releases are committed under `static/firmware/releases/`, which the web flasher reads.
Release notes say what changed for someone flashing the device, not what changed in the code.

A new core or modules release is not used by any project until its `xewe.lock` is moved: bump the
template's pins in the same breath as the release when the template should ship it.
