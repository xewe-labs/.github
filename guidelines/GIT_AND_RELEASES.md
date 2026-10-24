# Git and releases

## Branches

`main` is always buildable. Work on a branch named for the change (`fix-upload-timeout`,
`module-relay`), and merge through a pull request. Direct commits to `main` are for the
maintainer's own small changes only.

## Commits

One topic per commit. The subject is in the imperative, lowercase, with no trailing period, and
under 72 characters:

```
add scheduler module
fix wifi reconnect after deep sleep
remove esptool installation prompt
```

Add a body when the reason is not obvious from the diff: what was wrong, what changed, what it
affects. A change that breaks projects until they re-run setup always needs one, such as a changed
`xewe.toml` key, a moved file or a changed generated layout. Say so in the message.

Never commit generated or installed files: `build/` (tools venv, fetched core and libraries, the
generated `build/modules/` with `modules.lock`, build output and release runs), `src/Modules.h`,
`run.sh` (written by `./setup.sh`), `.venv/` and caches. One generated file is committed:
`doc/modules.md` in `xewe-os-modules`. Firmware binaries are never committed. They are assets of
the GitHub Release (see [Release channels](#release-channels)). The full table is in
[`REPOSITORIES.md`](REPOSITORIES.md).

## Versions

| | Where the version lives | Format | Tag | Latest |
|---|---|---|---|---|
| `xewe-os-core` (XeWeCore) | `library.properties` → `version=`, mirrored into `library.json` and `XEWE_CORE_VERSION*` | `X.Y.Z` | `X.Y.Z` | 3.1.2 |
| `xewe-os-modules` | each module's `module.properties` → `version=` | `X.Y.Z` | `<module>/X.Y.Z` per module, `X.Y.Z` for the set | set 3.1.1; `led`, `led-web` and `wifi` 3.1.2 |
| `xewe-os-tools` | `pyproject.toml` → `version`, plus the locations its `scripts/release.sh` lists | `X.Y.Z` | `X.Y.Z` | 3.1.6 |
| `xewe-os` firmware | `xewe.toml` → `[project] version` | `X.Y.Z` | `X.Y.Z` | 3.1.0 |

Tags carry no `v` from 3.0.0 on, in every repository, and older `v` tags keep their names.

Versioning is semantic, read from the consumer's side. MAJOR is for when existing firmware,
sketches or scripts must change, MINOR for new capability that older callers ignore, and PATCH
for fixes. Each module declares the core range it builds against, for example
`requires_core=>=3.0.0,<4.0.0`. The template's version is independent of the others, and its
`xewe.toml` names one ref each for core, modules and tools.

**Builds never change a version.** The firmware version is `[project] version` in `xewe.toml`.
There is no build counter and no `version_state`.

**Versions are never bumped by hand.** Each repository's `scripts/release.sh` sets every version
location (see [Tags and releases](#tags-and-releases)).

## Refs: pinned to tags

A ref in `xewe.toml` is a tag, a branch, a commit SHA or `latest`, the newest commit of the
repository's default branch (development only).

All four repositories were tagged `3.0.0` together as one ecosystem generation, and each has been
released on its own since. The template pins core `3.1.2`, modules `wifi/3.1.2` and tools `3.1.6`,
and its `ci.yml` calls the tools workflows at the same tag as its `[tools] ref` (`@3.1.6`). A
project moves its refs to the newest tags with `xewe manifest update`. The order for a new
generation is tools first (the callers name it), then core and modules, then the template and the
projects, each moving its refs and callers to the new tags before its own tag.

A firmware release records exactly what it was built from: every `meta.json` carries
`core_ref`/`core_commit`, `modules_ref`/`modules_commit`, `tools_ref`/`tools_commit`, the
`libraries` with their commits and the `project_commit`.

Third-party libraries (`[libraries]`, the modules' `libraries.toml`) are always pinned to tags.

## Tags and releases

A tag is permanent. Registries reject a reused version, and projects pin to what you published in
`xewe.toml`. Never move or delete a published tag; release a new patch version instead. Every
release step below is run by a human.

`xewe-os-tools`, `xewe-os-modules` and `xewe-os` (and every firmware built from it) release with
one command, run from a clean `main` that is level with `origin/main`:

```sh
scripts/release.sh <version> --notes "First line." --notes "Second line."
```

The notes are one to three lines and must pass `xewe release-notes check`. `--notes-file FILE`
reads them from a file. Everything a run writes goes under `build/release/<version>/`, which is
ignored. The script runs six stages:

1. Preflight lists every problem before it refuses: the branch, a clean tree (untracked files
   included), `main` level with `origin/main`, the tag free here and on origin, valid notes, an
   ignored `build/` and a logged-in `gh`.
2. Tests set the version lines and record every check into a release card run.
3. `xewe release-card render` writes `release_card.md` from those results, or refuses.
4. Only the version files are committed, as `release <version>`.
5. The tag message is the notes, a blank line and the card. `main` is pushed before the tag.
6. The script waits for CI on the tag, whose card job checks the card against its own run. The
   GitHub Release then carries the card as an asset.

A run that stops before stage 4 puts the version lines back, and `--resume` continues a run that
stopped. `--ci-timeout <min>` (default 60) bounds the CI wait.

| Repository | What differs |
|---|---|
| `xewe-os-core` | Still two runs. `scripts/release.sh --prepare <version>` sets every version location, writes `releases/<version>/notes.txt` and `release_card.md` and makes no git change. Commit them as `prepare <version>` and push `main`. Then `scripts/release.sh <version>` (try `--dry-run` first) verifies the card, waits for CI on `HEAD`, tags, pushes the tag and creates the GitHub Release. The Arduino Library Manager picks up the new tag by itself, because the library is registered |
| `xewe-os-modules` | `scripts/release.sh <version> <module>...` tags each named module `<module>/<version>` with its own GitHub Release. `--all` releases every module and also tags the set `<version>`, which marks a commit where every module is released at `<version>` |
| `xewe-os-tools` | consumers then move `[tools] ref` and their workflow callers (`@X.Y.Z`) to the new tag |
| `xewe-os` | `--hardware c3` also flashes and runs the board tests on that chip; without it they are compiled, not run. The tag must equal `[project] version`. CI (`ci.yml` → `xewe-release.yml`) builds every image and publishes the GitHub Release (below). `X.Y.Z-<suffix>` tags are pre-releases |

Release notes say what changed for someone flashing the device, not what changed in the code.

A new core or modules tag is not used by a project whose refs are tags until its `xewe.toml` is
moved. Move the template's refs in the same breath as the release when the template should ship
it.

## Release channels

A firmware release built by CI from a tag is a GitHub Release, the canonical and permanent copy.
It holds one `.bin` per build, `firmware_map.csv` with one row per image, `firmware-<version>.tar.gz`
with the whole release folder and `release_card.md`. A flasher reads `firmware_map.csv` from the
release's download URL. Nothing is committed back to `main` after the tag. Pre-releases
(`X.Y.Z-<suffix>`) are marked as such on GitHub, so they never take the final version's place.

The tools workflow can still commit each release to a branch (`publish_branch: true`), but that
option is deprecated, because it puts binaries in git history. New callers leave it off.

How the workflows do this, the toolchain cache and the repository settings are in
[`xewe-os-tools/doc/ci.md`](https://github.com/xewe-labs/xewe-os-tools/blob/main/doc/ci.md).
