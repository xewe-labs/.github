# Repositories

Every repository belongs to one family. The family decides its name, its layout and how it is
released.

| Family | Name | Holds | Released as |
|---|---|---|---|
| Library | `xewe-library-<name>` | an Arduino library (`library.properties`, `src/`, `examples/`) | git tag `X.Y.Z`, Arduino Library Manager, PlatformIO |
| Module | `xewe-os-module-<slug>` | one XeWe OS feature, installed into firmware | nothing to publish; the registry points at `main` or a tag |
| Firmware | `xewe-os`, `xewe-led-os` | a flashable device firmware | git tag `v<version>` + GitHub release with binaries |
| Tooling | descriptive name (`xewe-os-build-toolchain`, `publish-arduino-library`) | scripts and generators used by the others | git tag `vX.Y.Z` when projects need to pin it |
| Registry | `xewe-os-modules` | `repositories.txt`, the list of module repos | nothing; `main` is what setup reads |

Names are lowercase with dashes, and the prefix says the family. Legacy `xewe-led-*` repositories
predate this and are archived rather than renamed.

## Every repository has

* `README.md` — see [`documentation.md`](documentation.md)
* `LICENSE.txt` — GPL-3.0-only, unless the repository states otherwise
* `.gitignore` — at minimum `.DS_Store`, `.idea/`, `.vscode/`, and whatever that family generates
* `main` as the default branch

Source files start with the header in [`license-header.txt`](license-header.txt).

## Family layouts

**Library** — Arduino layout at the repo root, one entry header `src/XeWe<Name>.h`, everything
else in a folder per class. The full rules, including what the publishing tool enforces, are in
[`publish-arduino-library/docs/library-rules.md`](https://github.com/xewe-labs/publish-arduino-library/blob/main/docs/library-rules.md).

**Module** — see [`modules.md`](modules.md).

**Firmware** — the sketch and `Config.h` at the root, a `setup.sh` that installs modules and the
toolchain, `src/modules/` for installed modules, `build/` for build data and the installed
toolchain, `static/` for released binaries and media.

**Tooling** — self-contained, with its own README explaining how a project consumes it. Tooling
never stores project data: it is copied or cloned into a project, and everything specific to that
project stays in the project.

## Repository settings

* **Public by default.** Keep a repository private only while it has no usable version, or when it
  holds credentials-adjacent material.
* **Description filled in**, one line, so the organization page reads well. Modules start theirs
  with `XeWe OS module:`.
* **Archive rather than delete** a repository that is no longer developed.

## Adding a repository

1. Pick the family and name from the table above.
2. Copy the closest existing repository of that family as the starting point, rather than starting
   empty — the layout, `.gitignore` and README structure come with it.
3. Add the license, the README and, for a module, the registry pull request.
4. If the new repository changes how a family works, update these guidelines in the same breath.
