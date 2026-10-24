# .github

The organization repository of [xewe-labs](https://github.com/xewe-labs): the public profile, the rules every repository follows, and the contributing, security and template files GitHub applies across the organization. It is for contributors and maintainers.

## Problem

XeWe OS is spread over four repositories (xewe-os, xewe-os-core, xewe-os-modules and xewe-os-tools) plus the firmware products built on them. Without one place for the shared rules:

- Each repository keeps its own copy of the commit, release and C++ rules, and the copies drift apart.
- A contributor arriving at any one repository can't tell which rules are local and which hold everywhere.
- Every repository needs its own contributing guide, security policy and issue templates.

## Solution

The rules that span repositories live here once, and every other repository links to them by URL. Three ideas carry it:

- GitHub reads `profile/README.md` as the organization's landing page. It also uses `CONTRIBUTING.md`, `SECURITY.md` and the issue and pull request templates for every repository that has no copy of its own.
- A rule describes what the repositories already do. When a repository needs to differ, either the rule changes or that repository's README says why it differs.
- A guideline is short and links to the repository that enforces the rule, so the detail lives in one place.

## See it

Every source file in the organization starts with the header from [`guidelines/license-header.txt`](guidelines/license-header.txt), with its own path on the third line:

```cpp
// SPDX-FileCopyrightText: 2026 Maxim Dokukin (maxdokukin.com)
// SPDX-License-Identifier: GPL-3.0-only
// xewe-os-core/src/XeWeCore.h
```

## Why not a copy per repository

| | Rules here, linked | A copy in every repository |
|---|---|---|
| Changing a rule | one pull request | one pull request per repository |
| Drift | none, there is one copy | each copy ages on its own |
| Contributing, security and templates | inherited by every repository | written and kept up per repository |

## Install

Nothing to install. A new repository in the organization inherits `CONTRIBUTING.md`, `SECURITY.md` and the templates as soon as it exists. Link to a guideline by its full URL:

```text
https://github.com/xewe-labs/.github/blob/main/guidelines/GIT_AND_RELEASES.md
```

## Usage

Look up a rule in the guidelines:

| Document | Covers |
|---|---|
| [`guidelines/REPOSITORIES.md`](guidelines/REPOSITORIES.md) | what belongs in which repository, naming, settings |
| [`guidelines/CPP_STYLE.md`](guidelines/CPP_STYLE.md) | C++ layout, naming, includes, formatting |
| [`guidelines/GIT_AND_RELEASES.md`](guidelines/GIT_AND_RELEASES.md) | branches, commits, versions, tags |
| [`guidelines/DOCUMENTATION.md`](guidelines/DOCUMENTATION.md) | what every README says, where documentation belongs |

A rule that belongs to one tool stays with that tool. The module contract is in [`xewe-os-modules/doc/CONTRACT.md`](https://github.com/xewe-labs/xewe-os-modules/blob/main/doc/CONTRACT.md), the CI workflows are in [`xewe-os-tools/doc/ci.md`](https://github.com/xewe-labs/xewe-os-tools/blob/main/doc/ci.md), and the reasons behind the ecosystem are in [`xewe-os/doc/XEWE_OS_ECOSYSTEM.md`](https://github.com/xewe-labs/xewe-os/blob/main/doc/XEWE_OS_ECOSYSTEM.md).

To change a rule, open a pull request here.

## Status

The profile, the guidelines, the license header, `CONTRIBUTING.md`, `SECURITY.md`, the pull request template and two issue templates (bug report and module proposal) are in use. There are no workflow templates, because each repository holds a short caller of the reusable workflows in xewe-os-tools.

---

## Development

There is nothing to build or test. Before renaming a file, search the other xewe-labs repositories for its URL, since they link here.

## Layout

- [`profile/`](profile/README.md) holds the organization's landing page and its ecosystem diagram.
- [`guidelines/`](guidelines/) holds the organization rules and the license header.
- [`ISSUE_TEMPLATE/`](ISSUE_TEMPLATE/) holds the bug report and module proposal templates.
- `CONTRIBUTING.md`, `SECURITY.md` and `PULL_REQUEST_TEMPLATE.md` are the defaults GitHub applies to every repository without its own copy.

## License

GPL-3.0-only, see [LICENSE.txt](LICENSE.txt).
