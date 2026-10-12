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
generated `build/modules/` with `modules.lock`, build output), `src/Modules.h`, `run.sh` (written by
`./setup.sh`), `.venv/`, caches. One exception is generated but committed: `doc/modules.md` in
`xewe-os-modules`. Firmware binaries are never committed by hand: the template's release CI commits
them, in Git LFS (see [Release channels](#release-channels)). The full table is in
[`repositories.md`](repositories.md).

## Versions

| | Where the version lives | Format | Tag | Released |
|---|---|---|---|---|
| `xewe-os-core` (XeWeCore) | `library.properties` → `version=`, mirrored into `library.json` and `XEWE_CORE_VERSION*` | `X.Y.Z` | `X.Y.Z` | 3.0.0 |
| `xewe-os-modules` | each module's `module.properties` → `version=` | `X.Y.Z` | `<module>/X.Y.Z` per module, `X.Y.Z` for the set | 3.0.0 (buttons, pins, scheduler, time, web-interface, wifi, and the set) |
| `xewe-os-tools` | `pyproject.toml` → `version`, plus the locations its `scripts/release.sh` lists | `X.Y.Z` | `X.Y.Z` | 3.0.0, 3.0.1 |
| `xewe-os` firmware | `xewe.toml` → `[project] version` | `X.Y.Z` | `X.Y.Z` | 3.0.0 |

Tags carry no `v` from 3.0.0 on, in every repository; older `v` tags keep their names.

Semantic versioning, read from the consumer's side: MAJOR when existing firmware, sketches or
scripts must change, MINOR for new capability that older callers ignore, PATCH for fixes. Each
module declares the core range it builds against, for example `requires_core=>=3.0.0,<4.0.0`.
The template's version is independent of the others; its `xewe.toml` names one ref each for core,
modules and tools.

**Builds never change a version.** The firmware version is `[project] version` in `xewe.toml`.
There is no build counter and no `version_state`.

**Versions are never bumped by hand.** `scripts/release.sh --prepare <version>` sets every version
location of a repository (see [Tags and releases](#tags-and-releases)).

## Refs: pinned to tags

A ref in `xewe.toml` is a tag, a branch, a commit SHA or `latest`, the newest commit of the
repository's default branch (development only).

The four repositories were tagged `3.0.0` together, as one ecosystem generation; tools `3.0.1`
followed for the template's release commit. The template pins core `3.0.0`, the modules set tag
`3.0.0` and tools `3.0.1`, and its `ci.yml` calls the tools workflows at the same tag as its
`[tools] ref` (`@3.0.1`). A project moves its refs to the newest tags with `xewe manifest update`.
Order for a new generation: tools first (the callers name it), then core and modules, then the
template and the projects, each moving its refs and callers to the new tags before its own tag.

A firmware release records exactly what it was built from: every `meta.json` carries
`core_ref`/`core_commit`, `modules_ref`/`modules_commit`, `tools_ref`/`tools_commit`, the
`libraries` with their commits and the `project_commit`.

Third-party libraries (`[libraries]`, the modules' `libraries.toml`) are always pinned to tags.

## Tags and releases

A tag is permanent: registries reject a reused version, and projects pin to what you published in
`xewe.toml`. Never move or delete a published tag; release a new patch version instead. Agents
never tag, push or publish (`scripts/release.sh --dry-run` is fine); every release step below is
run by a human.

Every repository releases the same way, with its own `scripts/release.sh`:

1. `scripts/release.sh --prepare <version>` sets every version location and creates
   `releases/<version>/notes.txt` (the GitHub Release text) and `testcard.md` (what was tested on
   which commits, attached to the release) when they are missing. It makes no git change.
2. Fill both files, commit everything as `prepare <version>` and push `main`.
3. `scripts/release.sh <version>` (try `--dry-run` first) from that clean `main`, level with
   `origin/main`: 1 preflight (version `X.Y.Z`, branch, clean tree, tag free here and on origin,
   release files committed, every version location says the version, `gh` logged in); 2 waits for
   the CI workflow on HEAD to pass; 3 local checks; 4 tags HEAD `<version>` (annotated) and pushes
   it; 5 publishes or checks the result. `--ci-timeout <min>` (default 30) bounds the CI wait;
   `--skip-ci` skips it, for emergencies only. Re-running resumes: a tag without its GitHub Release
   only gets the release.

| Repository | Local checks (stage 3) | Stages 4 and 5 |
|---|---|---|
| `xewe-os-core` | `tests/unit/run.sh`, `xewe format --check` | creates the GitHub Release; the Arduino Library Manager picks the new tag up by itself (the library is registered) |
| `xewe-os-modules` | `tools/validate.py`, the module's unit tests | per module: `--prepare <version> <module>`, then `scripts/release.sh <version> <module>` tags `<module>/<version>` with its own GitHub Release (files in `modules/<module>/releases/<version>/`). After every module: `--prepare <version> --set`, then `scripts/release.sh <version> --set` checks every module is released at `<version>` and tags the set `<version>`, which projects pin and the tools fetch |
| `xewe-os-tools` | the pytest suite | creates the GitHub Release; consumers move `[tools] ref` and their workflow callers (`@X.Y.Z`) to it |
| `xewe-os` (and every firmware built from it) | `xewe test --no-board`, `xewe build` for c3 | the tag message is `notes.txt`; the tag must equal `[project] version`. CI (`ci.yml` → `xewe-release.yml`) builds and publishes the release (below); the script waits for that run, attaches `testcard.md` and checks the assets and the release commit. `X.Y.Z-<suffix>` tags are pre-releases |

Release notes say what changed for someone flashing the device, not what changed in the code.

A new core or modules tag is not used by a project whose refs are tags until its `xewe.toml` is
moved: move the template's refs in the same breath as the release when the template should ship it.

## Release channels

A firmware release built by CI from a tag goes to two places.

| Channel | What it holds | Who reads it |
|---|---|---|
| **GitHub Release** (always; the canonical copy) | one `.bin` per build, `firmware-<version>.tar.gz` (the whole release folder: binaries, `manifest.json`, `meta.json`, notes), and notes ending in a "Built from" list of commits | people and scripts; permanent |
| **`releases_branch`** (`publish_branch: true`; the template uses `main`) | `static/firmware/releases/<version>/` exactly as `xewe release` lays it out (`<chip>/<version>-<chip>-<name>.bin`, `manifest.json`, `meta.json`, plus `firmware_map.csv` and `release_notes.txt`), and `index.json` listing every version, newest first | the web flasher: static files at stable URLs |

The template builds the default matrix (c3, c6, s3). On `main`, the release arrives as one bot
commit after the tag, `release <version> (<tag>, <sha7>) [skip ci]`, that adds only the release
folder and `index.json`, so the tag itself does not hold its binaries. The `.bin` files are stored
in Git LFS: the template's `.gitattributes` routes `static/**/*.bin` there, and the job fails if a
`.bin` would be committed as a plain blob. Clone or pull with Git LFS installed, or the `.bin`
files stay pointer files. A project can instead keep the default orphan `releases` branch, written
only by CI and never fetched by `main` clones. Pre-releases (`X.Y.Z-<suffix>`) are marked as such
on GitHub and land in a folder `X.Y.Z-<suffix>`, so they never take the final version's place.
Settings: leave the releases branch unprotected or give GitHub Actions a bypass, and block
force-pushes and deletion there.

How the workflows do this, the toolchain cache and the repository settings are in
[`xewe-os-tools/doc/ci.md`](https://github.com/xewe-labs/xewe-os-tools/blob/main/doc/ci.md).
