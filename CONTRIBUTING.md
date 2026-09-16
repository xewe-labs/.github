# Contributing to XeWe Labs

This applies to every repository under [xewe-labs](https://github.com/xewe-labs). A repository
with its own `CONTRIBUTING.md` overrides this one.

## Before you start

* **A module?** You do not need access to this organization. Build it in your own repository and
  add it to the registry: [xewe-os-modules](https://github.com/xewe-labs/xewe-os-modules), rules in
  [`guidelines/modules.md`](https://github.com/xewe-labs/.github/blob/main/guidelines/modules.md).
* **A change to existing code?** Open an issue first when the change is large, changes behaviour
  people depend on, or touches more than one repository. Small fixes can go straight to a pull
  request.

## Working on a change

1. Clone the repositories you need side by side in one folder. The tooling finds them that way:
   `xewe-os` installs modules from local clones with `--modules-source ..`, and the publishing
   tool discovers libraries as sibling folders.
2. Branch off `main`.
3. Follow [`guidelines/cpp-style.md`](https://github.com/xewe-labs/.github/blob/main/guidelines/cpp-style.md) for C++ and
   [`guidelines/git-and-releases.md`](https://github.com/xewe-labs/.github/blob/main/guidelines/git-and-releases.md) for commits and versions.
4. Every source file starts with the header in
   [`guidelines/license-header.txt`](https://github.com/xewe-labs/.github/blob/main/guidelines/license-header.txt).
5. Update the README in the same pull request as the change it describes.

## Before opening a pull request

* **It builds.** Firmware and modules compile for ESP32-C3, C6 and S3 — modules with their own
  `scripts/validate.sh`, firmware with `build/scripts/<platform>/build.sh -c <chip>`. Libraries
  compile their examples for the same three boards.
* **It is formatted.** `build/scripts/<platform>/format.sh` from a project that has the toolchain
  installed, or `format.sh --check` to only report.
* **Generated files are not committed.** Installed modules, the installed toolchain, `build/libraries/`,
  `builds/`, `.venv/` and `build_config` stay out of git. See each repository's `.gitignore`.
* **Say what you tested.** Which boards, which commands, what you did not test.

## Review

One approving review merges. Keep pull requests to one topic; a change that spans repositories
gets one pull request per repository, and the description links the others.
