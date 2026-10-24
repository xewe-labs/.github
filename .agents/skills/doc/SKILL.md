---
name: doc
description: >
  What an agent needs to know about the xewe-labs organization that the code does not show:
  the five repositories and which one a change belongs in, what never to run without being asked
  (release.sh, flashing, arduino-cli, ~/.arduino15, xewe.toml edits, repository changes), C++
  and module rules that break silently (the XeWeCore include, the host parameter, [this]
  captures, the cli( macro, generated files), working with no board attached (XEWE_NO_BOARD,
  "compiled, not run"), running the xewe tools and a harness from local checkouts, the files
  GitHub reads by fixed name in this .github repository and the guideline URLs other repositories
  link to, and the decisions log. Use it before working in any xewe-labs repository, before a
  build, test, flash or release, before renaming a file here, and when asking why something is
  the way it is.
---

# doc

This is the index of the organization's agent knowledge. Open only the file whose row matches
the task. Each row names one file under `references/`, and every file there has a row.

| File | Read it when |
|---|---|
| `references/ECOSYSTEM.md` | starting work in any xewe-labs repository, or deciding which repository a change belongs in |
| `references/NEVER.md` | about to release, flash, run arduino-cli, edit `xewe.toml`, change a repository, or run `rm -rf` |
| `references/CODE_RULES.md` | writing C++ for the sketch, the core or a module, editing a generated file, or writing a comment |
| `references/NO_BOARD.md` | no board is attached, a command prints `compiled, not run`, or reporting test results |
| `references/COMMANDS.md` | running the `xewe` tools, choosing chips, or building against local checkouts |
| `references/GITHUB_FILES.md` | renaming or moving any file in this repository, or editing the profile or the templates |
| `references/DECISIONS.md` | changing anything a past decision covers |
