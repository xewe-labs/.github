# .github

The organization repository of [xewe-labs](https://github.com/xewe-labs): the public profile, the
rules every repository follows, and the contributing, security and template files GitHub applies
across the organization. It is for anyone who contributes to or maintains a xewe-labs repository.

## Problem

XeWe OS is spread over four repositories plus the firmware products built on them. Without one
place for the shared rules:

* each repository keeps its own copy of the commit, release and C++ rules, and the copies drift
  apart;
* a contributor arriving at any one repository can't tell which rules are local and which hold
  everywhere;
* every repository needs its own contributing guide, security policy and issue templates.

## Solution

The rules that span repositories live here once, and every other repository links to them by URL.
Rules that belong to one tool stay with that tool and are linked from here. Three ideas carry it:

1. GitHub reads `profile/README.md` as the organization's landing page, and uses
   `CONTRIBUTING.md`, `SECURITY.md` and the issue and pull request templates for every repository
   that has no copy of its own.
2. A rule describes what the repositories already do. When a repository needs to differ, either
   the rule changes or that repository's README says why it differs.
3. Everything else here is ordinary documentation, and `guidelines/` is the single place it lives.

## See it

Every source file in the organization starts with the header from
[`guidelines/license-header.txt`](guidelines/license-header.txt), with its own path on the third
line:

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

A repository can still override a file GitHub inherits by carrying its own copy.

## Install

Nothing to install. A new repository in the organization inherits `CONTRIBUTING.md`,
`SECURITY.md` and the templates as soon as it exists. Link to a guideline by its full URL, for
example:

```
https://github.com/xewe-labs/.github/blob/main/guidelines/GIT_AND_RELEASES.md
```

## Usage

Look up a rule in the guidelines:

| Document | Covers |
|---|---|
| [`guidelines/REPOSITORIES.md`](guidelines/REPOSITORIES.md) | the four xewe-os repositories, what belongs where, naming, versions, committed vs generated |
| [`guidelines/CPP_STYLE.md`](guidelines/CPP_STYLE.md) | C++ layout, naming, includes, debug flags, formatting |
| [`guidelines/GIT_AND_RELEASES.md`](guidelines/GIT_AND_RELEASES.md) | branches, commit messages, versioning, tags, releases |
| [`guidelines/DOCUMENTATION.md`](guidelines/DOCUMENTATION.md) | what every README says, where documentation belongs |
| [`guidelines/MODULES.md`](guidelines/MODULES.md) | the module non-negotiables; the full contract is `xewe-os-modules/doc/CONTRACT.md` |
| [`guidelines/license-header.txt`](guidelines/license-header.txt) | the header every source file starts with |

Follow a rule that belongs to one tool where it lives:

* Arduino library rules: [`xewe-os-core/doc/LIBRARY_RULES.md`](https://github.com/xewe-labs/xewe-os-core/blob/main/doc/LIBRARY_RULES.md)
* Setup, build, flash, serial, test, format, release and web UI export: [`xewe-os-tools`](https://github.com/xewe-labs/xewe-os-tools) ([`doc/spec.md`](https://github.com/xewe-labs/xewe-os-tools/blob/main/doc/spec.md))
* CI workflows: [`xewe-os-tools/doc/ci.md`](https://github.com/xewe-labs/xewe-os-tools/blob/main/doc/ci.md)
* The module contract and how to add a module: [`xewe-os-modules/doc/CONTRACT.md`](https://github.com/xewe-labs/xewe-os-modules/blob/main/doc/CONTRACT.md)
* Why the ecosystem is shaped this way: [`xewe-os/doc/XEWE_OS_ECOSYSTEM.md`](https://github.com/xewe-labs/xewe-os/blob/main/doc/XEWE_OS_ECOSYSTEM.md)

Change a rule by opening a pull request here.

## Status

The profile, the five guidelines, the license header, `CONTRIBUTING.md`, `SECURITY.md`, the pull
request template and two issue templates (bug report, module proposal) are in use. There are no
workflow templates, because CI is reusable workflows in `xewe-os-tools/.github/workflows/` and each
repository holds a short caller.

---

## Development

There is nothing to build or test. Before renaming a file, search the other xewe-labs repositories
for its URL, since they link here.

## Layout

| Path | What it is |
|---|---|
| [`profile/`](profile/README.md) | the organization's landing page and its ecosystem diagram |
| [`guidelines/`](guidelines/) | the organization rules and the license header |
| [`ISSUE_TEMPLATE/`](ISSUE_TEMPLATE/) | the bug report and module proposal templates |
| [`CONTRIBUTING.md`](CONTRIBUTING.md), [`SECURITY.md`](SECURITY.md), [`PULL_REQUEST_TEMPLATE.md`](PULL_REQUEST_TEMPLATE.md) | the defaults GitHub applies to every repository without its own copy |

## License

This repository has no license file of its own. Every xewe-labs repository is GPL-3.0-only unless
it says otherwise.
