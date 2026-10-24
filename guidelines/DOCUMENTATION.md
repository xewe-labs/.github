# Documentation

Documentation lives next to what it describes, in the repository that owns it. A rule that spans
repositories lives in `.github`, and a rule about one tool lives with that tool.

Documentation is part of the change. A pull request that changes behaviour updates the README in
the same pull request.

Every xewe-labs repository follows the xewe_doc layout of the
[xewe workspace](https://github.com/maxdokukin/dot_agents). This page covers the part of it that
people read, plus the conventions specific to XeWe OS.

## Every repository's README

The root README is written for someone who arrived from a search result and doesn't know the
organization. Its first sentence says what the project is and who it is for, and the first
paragraph names the hardware or project it serves and links to the piece it plugs into.

The README tells the project's story in this order and stays under 500 lines:

| Section | What it holds |
|---|---|
| Title | the name, and one sentence on what it is and who it is for |
| Problem | two or three concrete faults of how things go today |
| Solution | one paragraph that resolves them, then the three ideas everything rests on |
| See it | the smallest real example, such as one command and its output |
| Why not X | an honest comparison with the obvious alternative |
| Install | the shortest path to a working result, as commands that can be pasted |
| Usage | the three things people actually do, with real examples, including the files someone edits and what happens when they do |
| Status | what works today and what comes next |

Below a horizontal rule come Development (how to change it and how to test it), Layout (one line
per top-level folder, linking to that folder's README and marking what is generated or installed)
and License (one line).

Conventions that keep the repositories readable together:

* **Commands go in fenced blocks, one per line**, with the flags someone actually needs and a
  short comment where it isn't obvious.
* **Tables are for items a reader compares or looks up**, such as commands, paths, metadata keys
  and where pieces come from.
* **Say what is generated.** Mark generated and installed paths, and name the script that creates
  them, so nobody edits a file that gets replaced.
* **Link across repositories** with full GitHub URLs, since these READMEs are read on their own.
* **No placeholder sections.** An empty "Roadmap" is worse than no section.
* **Markdown file names are uppercase** with underscores between words, such as `README.md` and
  `RELEASE_FLOW.md`. Folder names stay lowercase. A file GitHub reads by a fixed name keeps it.

Some READMEs have fixed contents. XeWeCore's README shows the one include (`<XeWeCore.h>`), a minimal sketch,
the `os.serial`/`os.cli`/`os.nvs`/`os.system` members and its dependency, with the full reference
in its `doc/`. Each module's `modules/<slug>/README.md` lists its commands in a table with the
`$<id>` prefix, its requirements and a Tests section (preconditions and the `xewe test --module`
line). The modules repo's own README says what a module is and how to add one. The tools README
lists the command surface. The template README covers clone, `./setup.sh` (and the module
selection made at first setup), `./run.sh` and the first-boot flow, the example modules, and what
is committed and what is generated.

The onboarding story has three levels, and every README that introduces XeWe OS tells it the same
way. Level 1 is XeWeCore from the Arduino Library Manager (example `01_Hello`), level 2 adds one
module of your own in the sketch folder (example `02_MyModule`), and level 3 is the `xewe-os`
template with ready-made modules and the `xewe` tools. XeWeCore's `doc/README.md` is the reference
text, and others link to it. Why the ecosystem is shaped the way it is goes in
`xewe-os/doc/XEWE_OS_ECOSYSTEM.md`, not in every README.

## Directory READMEs

A folder gets a `README.md` when a newcomer would otherwise have to read several files to learn
what it is for, usually `src/`, `tests/` and the level beneath them. It says what the folder turns
into what, which files to open first, and the command that runs or tests it. Most fit in 20
lines. A folder whose name and contents already explain it gets none. A change that alters what a
folder does or how it runs updates its README in the same change.

## Code comments

Code carries almost no comments. A one-line comment goes only where the code does something
surprising and renaming or restructuring cannot remove the surprise, such as an invariant, a
limit, a hardware or library fact, or a stored-data compatibility reason. Docstrings, comment
blocks, tutorials and commented-out code are removed, and anything in them worth keeping moves to
a README. Example code (core examples, the template's sample modules) is the exception, because it
explains the API to its reader.

## No process in code or docs

Code comments, READMEs and reference pages describe what is there now and why. They never carry
the process that produced them: no tool or session names, dates, decision or finding ids, run or
wave names, "was ..." or "used to" history, review, report or verification status, and no version
tags on features ("since 2.1"). Source files keep their SPDX and path header lines. History lives
in git, not in the repositories' files.

## The xewe workspace

Every repository carries the xewe workspace, which holds what a coding assistant needs and the
code does not show. Its layout and rules are in
[dot_agents](https://github.com/maxdokukin/dot_agents). Its files are short and prescriptive and
never repeat the README. READMEs and `doc/` are for people and hold no instructions for coding
assistants. No repository has a root `AGENTS.md` or `CLAUDE.md`.

## Where documentation belongs

| Kind | Where |
|---|---|
| How to use or build this repository | its root `README.md` |
| What one folder does and how to run or test it | that folder's `README.md` |
| Rules for everything in the organization | `.github/guidelines/` |
| Rules a tool enforces | with that tool or the repository it checks (for example `xewe-os-core/doc/LIBRARY_RULES.md`, checked by `arduino-lint`) |
| What a coding assistant needs and the code does not show | the repository's xewe workspace |
| A human guide too long for the README that belongs to no single folder | a `doc/` folder in the repository it belongs to, linked from that README |

Don't park documentation about one repository in another. Firmware docs that describe modules or
libraries belong in those repositories, with a link from the firmware README.
