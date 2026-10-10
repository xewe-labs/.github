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

* **A change that breaks projects until they re-run setup** — a changed `xewe.toml` key, a moved
  file, a changed generated layout. Say so in the message.
* **A change made mostly by an agent.** Keep the `Co-Authored-By` trailer that records it.

Never commit generated or installed files: `build/` (tools venv, fetched core and libraries, the
generated `build/modules/` with `modules.lock`, build output), `src/Modules.h`, `.venv/`, caches. Two
exceptions are generated but committed: `library.json` (registries read it from the repository)
and `MODULES.md` in `xewe-os-modules`. Firmware binaries are not committed to `main` either: CI
publishes them (see [Release channels](#release-channels)). The full table is in
[`repositories.md`](repositories.md).

## Versions

| | Where the version lives | Format | Tag |
|---|---|---|---|
| `xewe-os-core` (XeWeCore) | `library.properties` → `version=`, mirrored into `library.json` | `X.Y.Z` | `X.Y.Z`, un-prefixed |
| `xewe-os-modules` | the repo tag; each module's `module.properties` → `version=` is informational | `X.Y.Z` | `vX.Y.Z` |
| `xewe-os-tools` | `pyproject.toml` → `version` | `X.Y.Z` | `vX.Y.Z` |
| `xewe-os` firmware | `xewe.toml` → `[project] version` | `X.Y.Z` | `vX.Y.Z` |

Semantic versioning, read from the consumer's side: MAJOR when existing firmware, sketches or
scripts must change, MINOR for new capability that older callers ignore, PATCH for fixes. Each
module declares the core range it builds against, for example `requires_core=>=2.1.0,<3.0.0`.
The template's version is independent of the others; its `xewe.toml` names one ref each for core,
modules and tools.

**Builds never change a version.** The firmware version is `[project] version` in `xewe.toml`, and
only `xewe release --version X.Y.Z` writes it. There is no build counter and no `version_state`.

XeWeCore's version is bumped by the publishing tool, not by hand.

## Refs: `latest` now, tags from v3

A ref in `xewe.toml` is a tag, a branch, a commit SHA or `latest`, the newest commit of the
repository's default branch.

**Now (2.x):** the template and every project track `latest` for core, modules and tools, and the
callers of the shared CI workflows name `xewe-os-tools@main`. A firmware release still records
exactly what it was built from: every `meta.json` carries `core_ref`/`core_commit`,
`modules_ref`/`modules_commit`, `tools_ref`/`tools_commit`, the `libraries` with their commits and
the `project_commit`, so a `latest` build is reproducible from its release. Firmware projects may
publish 2.x releases and pre-releases (`v2.1.0`, `v2.0.0-rc1`); the libraries and tools are not
tagged in this phase.

**v3:** every repository is tagged `v3.0.0` together, as one ecosystem generation (core keeps
its un-prefixed `3.0.0`, see below), with the CI/CD definitions in place. From then on projects
freeze their refs at tags with `xewe manifest update`, and the CI callers move from `@main` to
`@v3.0.0`. Order: tools first (the callers name it), then core and modules, then the template and
the projects, each moving its refs and callers to the new tags before its own tag.

Third-party libraries (`[libraries]`, the modules' `libraries.toml`) are always pinned to tags.

## Tags and releases

A tag is permanent: registries reject a reused version, and projects pin to what you published in
`xewe.toml`. Never move or delete a published tag; release a new patch version instead. Agents
never tag, push or publish; every release step below is run by a human.

| Repository | How it is released |
|---|---|
| `xewe-os-core` | through [`publish-arduino-library`](https://github.com/xewe-labs/publish-arduino-library): `publish.py check xewe-os-core` (layout, metadata, `arduino-lint --compliance strict`, host tests, every example compiles), then `publish.py release xewe-os-core` (prints the plan), then `--execute` (or `scripts/publish.sh`), which bumps the version, tags `X.Y.Z` and creates the GitHub release. The Arduino Library Manager indexes the new tag by itself within about an hour once the library is registered; registering is a separate one-time step (`publish.py register`, or a registry change request when the URL or name of an existing entry changes). CI (`tests.yml`) runs the same lint and the examples on every push, but never publishes |
| `xewe-os-modules` | CI green (`tools/validate.py`, unit tests, a harness compile), then an annotated tag `vX.Y.Z` on `main`. Projects move to it with `xewe manifest update` |
| `xewe-os-tools` | CI green (pytest on 3.11–3.13, pyflakes, mypy, actionlint), then an annotated tag `vX.Y.Z`. The template's `[tools] ref` pins it, and the CI callers name it as `@vX.Y.Z` |
| `xewe-os` (and every firmware built from it) | set `[project] version` in `xewe.toml`, commit, push `main`, then push an annotated tag `vX.Y.Z` (the tag message is the release notes) or `vX.Y.Z-<suffix>` for a pre-release. CI runs `xewe release` on the tag and publishes the result (below). The tag must match `[project] version`, or the run fails before building. `xewe release` run locally is a rehearsal: it builds the same folder and prints the commands, nothing more |

Release notes say what changed for someone flashing the device, not what changed in the code.

A new core or modules tag is not used by a project whose refs are tags until its `xewe.toml` is
moved: move the template's refs in the same breath as the release when the template should ship it.

## Release channels

A firmware release built by CI from a tag goes to two places. Neither is `main`.

| Channel | What it holds | Who reads it |
|---|---|---|
| **GitHub Release** (always; the canonical copy) | one `.bin` per build, `firmware-<version>.tar.gz` (the whole release folder: binaries, `manifest.json`, `meta.json`, notes), and notes ending in a "Built from" list of commits | people and scripts; permanent, nothing added to git |
| **`releases` branch** (projects with `publish_branch: true`) | `static/firmware/releases/<version>/` exactly as `xewe release` lays it out, plus `index.json` listing every version, newest first | the web flasher: static files at stable URLs. An orphan branch written only by CI; `main` clones never fetch it |

Pre-releases (`vX.Y.Z-<suffix>`) are marked as such on GitHub and land in a folder
`X.Y.Z-<suffix>`, so they never take the final version's place. The template publishes GitHub
Releases only until its old `binaries` branch is retired or replaced. Settings: leave `releases`
unprotected or give GitHub Actions a bypass, and block force-pushes and deletion there.

How the workflows do this, the toolchain cache and the repository settings are in
[`xewe-os-tools/ci/README.md`](https://github.com/xewe-labs/xewe-os-tools/blob/main/ci/README.md).
