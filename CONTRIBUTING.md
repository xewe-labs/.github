# Contributing to XeWe Labs

This applies to every repository under [xewe-labs](https://github.com/xewe-labs). A repository
with its own `CONTRIBUTING.md` overrides this one.

## Before you start

* **A module?** You do not need access to this organization. Open a module proposal issue on
  [xewe-os-modules](https://github.com/xewe-labs/xewe-os-modules), then a pull request adding
  `modules/<slug>/` to it. Rules in
  [`doc/CONTRACT.md`](https://github.com/xewe-labs/xewe-os-modules/blob/main/doc/CONTRACT.md).
* **A change to existing code?** Open an issue first when the change is large, changes behaviour
  people depend on, or touches more than one repository. Small fixes can go straight to a pull
  request.

## Working on a change

1. Clone the repositories you need side by side in one folder, and work in a **copy** of the
   `xewe-os` template as the harness. Its setup takes local checkouts instead of the pinned refs:
   `./setup.sh --core-source ../xewe-os-core --modules-source ../xewe-os-modules`, and
   `XEWE_TOOLS_SOURCE=../xewe-os-tools` for the tools (`XEWE_CORE_SOURCE` and
   `XEWE_MODULES_SOURCE` are the environment forms). `./setup.sh` writes `./run.sh`, which is never
   committed.
2. Branch off `main`.
3. Follow [`guidelines/CPP_STYLE.md`](https://github.com/xewe-labs/.github/blob/main/guidelines/CPP_STYLE.md) for C++ and
   [`guidelines/GIT_AND_RELEASES.md`](https://github.com/xewe-labs/.github/blob/main/guidelines/GIT_AND_RELEASES.md) for commits and versions.
4. Every source file starts with the header in
   [`guidelines/license-header.txt`](https://github.com/xewe-labs/.github/blob/main/guidelines/license-header.txt).
5. Update the README in the same pull request as the change it describes.

## Before opening a pull request

Work through the checklist in the
[pull request template](https://github.com/xewe-labs/.github/blob/main/PULL_REQUEST_TEMPLATE.md).
It covers the build, formatting, tests and generated files, and CI runs the same checks. Say in
the description which boards and commands you tested and what you did not.

## Review

One approving review merges. Keep pull requests to one topic; a change that spans repositories
gets one pull request per repository, and the description links the others.
