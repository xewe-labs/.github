# Contributing to XeWe Labs

This applies to every repository under [xewe-labs](https://github.com/xewe-labs). A repository
with its own `CONTRIBUTING.md` overrides this one.

## Before you start

* **A module?** You do not need access to this organization. Open a module proposal issue on
  [xewe-os-modules](https://github.com/xewe-labs/xewe-os-modules), then a pull request adding
  `modules/<slug>/` to it. Rules in
  [`CONTRACT.md`](https://github.com/xewe-labs/xewe-os-modules/blob/main/CONTRACT.md), summarised in
  [`guidelines/modules.md`](https://github.com/xewe-labs/.github/blob/main/guidelines/modules.md).
* **A change to existing code?** Open an issue first when the change is large, changes behaviour
  people depend on, or touches more than one repository. Small fixes can go straight to a pull
  request.

## Working on a change

1. Clone the repositories you need side by side in one folder, and work in a **copy** of the
   `xewe-os` template as the harness. Its setup takes local checkouts instead of the pinned refs:
   `./setup.sh --core-source ../xewe-os-core --modules-source ../xewe-os-modules`, and
   `XEWE_TOOLS_SOURCE=../xewe-os-tools` for the tools (`XEWE_CORE_SOURCE` and
   `XEWE_MODULES_SOURCE` are the environment forms). The publishing tool discovers the library
   as a sibling folder.
2. Branch off `main`.
3. Follow [`guidelines/cpp-style.md`](https://github.com/xewe-labs/.github/blob/main/guidelines/cpp-style.md) for C++ and
   [`guidelines/git-and-releases.md`](https://github.com/xewe-labs/.github/blob/main/guidelines/git-and-releases.md) for commits and versions.
4. Every source file starts with the header in
   [`guidelines/license-header.txt`](https://github.com/xewe-labs/.github/blob/main/guidelines/license-header.txt).
5. Update the README in the same pull request as the change it describes.

## Before opening a pull request

* **It builds.** Everything compiles for ESP32-C3, C6 and S3 with 0 warnings, locally (there is
  no CI): firmware with `build/tools/.venv/bin/python -m xewe build --all-chips`, a module with
  `xewe test --module <slug> --all-chips` in a harness plus `tools/validate.py`, XeWeCore with
  `publish.py check xewe-os-core` and `tests/unit/run.sh`, the tools with their pytest suite.
* **It is formatted** to [`guidelines/cpp-style.md`](https://github.com/xewe-labs/.github/blob/main/guidelines/cpp-style.md).
* **Generated files are not committed.** `build/`, `src/Modules.h`, `.venv/` and
  caches stay out of git. See each repository's `.gitignore`.
* **Say what you tested.** Which boards, which commands, what you did not test.

## Review

One approving review merges. Keep pull requests to one topic; a change that spans repositories
gets one pull request per repository, and the description links the others.
