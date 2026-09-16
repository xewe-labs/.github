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
| [`guidelines/repositories.md`](guidelines/repositories.md) | repository families, naming, required files, visibility |
| [`guidelines/cpp-style.md`](guidelines/cpp-style.md) | C++ layout, naming, includes, debug flags, formatting |
| [`guidelines/git-and-releases.md`](guidelines/git-and-releases.md) | branches, commit messages, versioning, tags, releases |
| [`guidelines/documentation.md`](guidelines/documentation.md) | what every README says, where documentation belongs |
| [`guidelines/modules.md`](guidelines/modules.md) | the XeWe OS module contract |
| [`AGENTS.md`](AGENTS.md) | rules for coding agents working in any xewe-labs repository |
| [`guidelines/license-header.txt`](guidelines/license-header.txt) | the header every source file starts with |

Rules that belong to one tool stay with that tool and are linked from here:

* Arduino library rules: [`publish-arduino-library/docs/library-rules.md`](https://github.com/xewe-labs/publish-arduino-library/blob/main/docs/library-rules.md)
* Build, flash, release and format scripts: [`xewe-os-build-toolchain`](https://github.com/xewe-labs/xewe-os-build-toolchain)
* Module registry and how to add a module: [`xewe-os-modules`](https://github.com/xewe-labs/xewe-os-modules)

## Changing a rule

Open a pull request here. A rule describes what the repositories already do; when a repository
needs to differ, either change the rule or say in that repository's README why it differs.
