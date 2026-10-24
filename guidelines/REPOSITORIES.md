# Repositories

XeWe OS is four repositories: `xewe-os`, `xewe-os-core`, `xewe-os-modules` and `xewe-os-tools`.
This `.github` repository is separate and holds the organization's rules. Each repository has one
job, and code goes where its job is. What each one holds, and why the split is drawn this way, is
in
[`XEWE_OS_ECOSYSTEM.md`](https://github.com/xewe-labs/xewe-os/blob/main/doc/XEWE_OS_ECOSYSTEM.md).

## Where code goes

* **Would a second firmware need it?** Then it is core (if every firmware needs it) or a module
  (if some do), never template code.
* **Does it run on the host, not the chip?** It is tools. The modules repository and the template
  carry no toolchain of their own, and modules are built and tested through an `xewe-os` checkout.
* **A project built from the template** keeps its own code in the sketch and `Config.h`. A feature
  that turns out reusable moves into the modules repository as a pull request.

CI is one set of reusable workflows in `xewe-os-tools`, called by a short workflow in each
repository. They are documented in
[`ci.md`](https://github.com/xewe-labs/xewe-os-tools/blob/main/doc/ci.md).

## Naming

Repository names are lowercase with dashes. Everything that is part of XeWe OS is prefixed
`xewe-os-*` (the template itself is `xewe-os`). Modules are folders, not repositories:
`modules/<slug>/` in `xewe-os-modules`. A firmware built from the template is named for the
device (for example `xewe-led-os`). Older `xewe-led-*` repositories predate this and are archived
rather than renamed.

## Committed and generated

Nothing generated is committed, and there are no submodules. The template's dependencies come
from the refs in `xewe.toml`, fetched by `./setup.sh`. The list of generated paths is in
[`XEWE_OS_ECOSYSTEM.md`](https://github.com/xewe-labs/xewe-os/blob/main/doc/XEWE_OS_ECOSYSTEM.md),
and each repository's `.gitignore` enforces it. Two generated files are committed on purpose:
`doc/modules.md` in `xewe-os-modules`, written by `tools/validate.py --write-index`, and
`library.json` in `xewe-os-core`, which registries read and which carries the version of
`library.properties`. Firmware binaries are never committed. They are assets of a GitHub Release
(see [`GIT_AND_RELEASES.md`](GIT_AND_RELEASES.md#release-channels)).

## Every repository has

* `README.md`, see [`DOCUMENTATION.md`](DOCUMENTATION.md)
* the xewe workspace, see [`DOCUMENTATION.md`](DOCUMENTATION.md#the-xewe-workspace)
* `LICENSE.txt`, GPL-3.0-only unless the repository states otherwise
* `.gitignore`, at minimum `.DS_Store`, `.idea/`, `.vscode/` and whatever that repository
  generates
* `main` as the default branch

Source files start with the header in [`license-header.txt`](license-header.txt).

## Repository settings

* **Public by default.** Keep a repository private only while it has no usable version, or when it
  holds credentials-adjacent material.
* **Description filled in**, one line, so the organization page reads well.
* `xewe-os` is marked as a **template repository**, and also works as a plain `git clone`.
* **Archive rather than delete** a repository that is no longer developed.

## Adding a repository

The split is deliberate, and a new `xewe-os-*` repository needs a reason none of the four can
hold. A new module is not a repository: open a module proposal and a pull request to
`xewe-os-modules`, following its
[contract](https://github.com/xewe-labs/xewe-os-modules/blob/main/doc/CONTRACT.md). A new firmware
product is a project created from the `xewe-os` template. If a new repository changes how the
ecosystem works, update these guidelines and `xewe-os/doc/XEWE_OS_ECOSYSTEM.md` in the same change.
