# .github

Organization-wide rules, guidelines and documentation for
[xewe-labs](https://github.com/xewe-labs).

GitHub treats this repository specially: `profile/README.md` is the organization's landing page,
and `CONTRIBUTING.md`, `SECURITY.md` and issue/pull request templates here are used by every
repository in the organization that does not have its own copy. Everything else in this repository
is ordinary documentation: it is the single place the rules live, and other repositories link to it.

## Guidelines

| Document | Covers |
|---|---|
| [`guidelines/repositories.md`](guidelines/repositories.md) | the four xewe-os repositories, what belongs where, naming, versions, committed vs generated |
| [`guidelines/cpp-style.md`](guidelines/cpp-style.md) | C++ layout, naming, includes, debug flags, formatting |
| [`guidelines/git-and-releases.md`](guidelines/git-and-releases.md) | branches, commit messages, versioning, tags, releases |
| [`guidelines/documentation.md`](guidelines/documentation.md) | what every README says, where documentation belongs |
| [`guidelines/modules.md`](guidelines/modules.md) | the module non-negotiables; the full contract is `xewe-os-modules/CONTRACT.md` |
| [`AGENTS.md`](AGENTS.md) | rules for coding agents working in any xewe-labs repository |
| [`guidelines/license-header.txt`](guidelines/license-header.txt) | the header every source file starts with |

Rules that belong to one tool stay with that tool and are linked from here:

* Arduino library rules: [`publish-arduino-library/docs/library-rules.md`](https://github.com/xewe-labs/publish-arduino-library/blob/main/docs/library-rules.md)
* Setup, build, flash, serial, test and release: [`xewe-os-tools`](https://github.com/xewe-labs/xewe-os-tools) (`SPEC.md`)
* The module contract and how to add a module: [`xewe-os-modules/CONTRACT.md`](https://github.com/xewe-labs/xewe-os-modules/blob/main/CONTRACT.md)
* Why the ecosystem is shaped this way: [`xewe-os/ARCHITECTURE.md`](https://github.com/xewe-labs/xewe-os/blob/main/ARCHITECTURE.md)

Issue and pull request templates: [`ISSUE_TEMPLATE/`](ISSUE_TEMPLATE/) (bug report, module
proposal) and [`PULL_REQUEST_TEMPLATE.md`](PULL_REQUEST_TEMPLATE.md). There are no workflow
templates: CI is intentionally absent for now, and builds run locally through `xewe-os-tools`.

## Changing a rule

Open a pull request here. A rule describes what the repositories already do; when a repository
needs to differ, either change the rule or say in that repository's README why it differs.
