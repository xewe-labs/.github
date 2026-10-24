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
`xewe.toml` key, a moved file or a changed generated layout.

Never commit generated or installed files (`build/`, `src/Modules.h`, `run.sh`, `.venv/`, caches)
or firmware binaries. What is generated in each repository is listed in
[`XEWE_OS_ECOSYSTEM.md`](https://github.com/xewe-labs/xewe-os/blob/main/doc/XEWE_OS_ECOSYSTEM.md),
and binaries are assets of the GitHub Release.

## Versions

Every repository uses `X.Y.Z`. Where each version is written, and which release it is on now, is
in that repository: the `version=` line of `library.properties` in
[`xewe-os-core`](https://github.com/xewe-labs/xewe-os-core), of each `module.properties` in
[`xewe-os-modules`](https://github.com/xewe-labs/xewe-os-modules), `pyproject.toml` in
[`xewe-os-tools`](https://github.com/xewe-labs/xewe-os-tools), and `[project] version` in the
`xewe.toml` of [`xewe-os`](https://github.com/xewe-labs/xewe-os).

Versioning is semantic, read from the consumer's side. MAJOR is for when existing firmware,
sketches or scripts must change, MINOR for new capability that older callers ignore, and PATCH
for fixes. Each module declares the core range it builds against in `requires_core`. The
template's version is independent of the others.

Tags are the version with no `v` from 3.0.0 on, in every repository, and older `v` tags keep their
names. A module is tagged `<module>/X.Y.Z`, and the modules repository also tags the whole set
`X.Y.Z`. A tag `X.Y.Z-<suffix>` is a pre-release.

Builds never change a version, and versions are never bumped by hand. Each repository's
`scripts/release.sh` sets every version location.

## Refs

A ref in `xewe.toml` is a tag, a branch, a commit SHA or `latest`, the newest commit of the
default branch (development only). Releases pin tags. The template's `ci.yml` calls the tools
workflows at the same tag as its `[tools] ref`, and `xewe manifest update` moves a project's refs
to the newest tags. A new ecosystem generation moves in this order: tools first, because the
callers name it, then core and modules, then the template and the projects, each updating its refs
and callers before its own tag. Third-party libraries are always pinned to tags. Every
`meta.json` of a firmware release records the refs and commits it was built from.

## Tags and releases

A tag is permanent. Registries reject a reused version, and projects pin to what you published.
Never move or delete a published tag; release a new patch version instead. A human runs every
release.

Each repository releases with its own `scripts/release.sh`, run from a clean `main` level with
`origin/main`. The stages, flags and per-repository differences are in its `scripts/README.md`:
[`xewe-os`](https://github.com/xewe-labs/xewe-os/blob/main/scripts/README.md),
[`xewe-os-core`](https://github.com/xewe-labs/xewe-os-core/blob/main/scripts/README.md),
[`xewe-os-modules`](https://github.com/xewe-labs/xewe-os-modules/blob/main/scripts/README.md) and
[`xewe-os-tools`](https://github.com/xewe-labs/xewe-os-tools/blob/main/scripts/README.md).
Release notes are one to three lines and must pass `xewe release-notes check`.

Release notes say what changed for someone flashing the device, not what changed in the code.

A new core or modules tag is not used by a project whose refs are tags until its `xewe.toml` is
moved, so move the template's refs together with the release when the template should ship it.

## Release channels

A firmware release built by CI from a tag is a GitHub Release, the canonical and permanent copy.
Nothing is committed back to `main` after the tag. How the workflows build and publish it is in
[`ci.md`](https://github.com/xewe-labs/xewe-os-tools/blob/main/doc/ci.md).
