---
name: Module proposal
about: Propose a new module for xewe-os-modules before opening the pull request
title: "module: <slug>"
labels: module
---

<!-- A module is code someone figured out once so nobody has to figure it out again.
The rules are in xewe-os-modules/CONTRACT.md; this proposal checks the identity and the
dependencies before any code is written. -->

**What it figures out once**
<!-- The problem this module solves, in two or three sentences: the hardware, protocol or
library quirk that took effort, and what a firmware gets by selecting it. -->

**Identity**

| Key | Value |
|---|---|
| `slug` (folder, `--modules` value; `[a-z0-9-]`) | |
| `id` (CLI group `$<id>` and NVS namespace; `[a-z][a-z0-9_]*`, at most 15 characters, never changes) | |
| `name` / class / `folder` (`[A-Z][A-Za-z0-9]*`) | |
| `declare` line | `<Folder> <var>(os[, <dep var>...]);` |

**Dependencies**
- `depends_modules`: <!-- slugs of existing modules, or none -->
- `depends_libraries`: <!-- third-party Arduino libraries, pinned in the modules repo's
  libraries.toml; not esp32-core libraries (WiFi, Wire, ...), not XeWeCore -->
- `requires_core`: <!-- usually >=2.1.0,<3.0.0 -->

**Commands**

| Command | What it does |
|---|---|
| `$<id> status` | required: `<name> module (enabled\|disabled)` plus the module's state |
| | |

**Tests**
<!-- The behaviour test you will write beside test_compiles and test_status: which command, what
output it expects, and the board preconditions (wiring, env vars such as XEWE_TEST_<ID>_<WHAT>). -->

**Hardware**
<!-- Chips you can test on (c3/c6/s3), wiring, pins. -->
