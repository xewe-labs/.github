# Documentation

Documentation lives next to what it describes, in the repository that owns it. A rule that spans
repositories lives in `.github`, and a rule about one tool lives with that tool.

Documentation is part of the change. A pull request that changes behaviour updates the README in
the same pull request.

Every xewe-labs repository follows the xewe_doc layout of the
[xewe workspace](https://github.com/maxdokukin/dot_agents), and where this page and xewe_doc
disagree, xewe_doc wins. This page covers the part people read, plus the conventions specific to
XeWe OS.

## Every repository's README

The root README is written for someone who arrived from a search result and doesn't know the
organization. Its first sentence says what the project is and who it is for. It tells the
project's story in this order and stays under 500 lines:

| Section | What it holds |
|---|---|
| Title | the name, and one sentence on what it is and who it is for |
| Problem | two or three concrete faults of how things go today |
| Solution | one paragraph that resolves them, then the three ideas everything rests on |
| See it | the smallest real example, such as one command and its output |
| Why not X | an honest comparison with the obvious alternative |
| Install | the shortest path to a working result, as commands that can be pasted |
| Usage | the three things people actually do, with real examples |
| Status | what works today and what comes next |

Below a horizontal rule come Development (how to change it and how to test it), Layout (one line
per top-level folder, linking to that folder's README) and License (one line).

* **Commands go in fenced blocks, one per line**, with the flags someone needs.
* **Tables are for items a reader compares or looks up**, such as commands, paths and keys.
* **Say what is generated.** Mark generated and installed paths and name the script that creates
  them, so nobody edits a file that gets replaced.
* **Link across repositories** with full GitHub URLs, since these READMEs are read on their own.
* **No placeholder sections.** An empty "Roadmap" is worse than no section.
* **Markdown file names are uppercase** with underscores between words. Folder names stay
  lowercase. A file GitHub reads by a fixed name keeps it.

Some READMEs have fixed contents. XeWeCore's shows the one include (`<XeWeCore.h>`), a minimal
sketch and its dependency, and leaves the reference to its `doc/`, whose
[`README.md`](https://github.com/xewe-labs/xewe-os-core/blob/main/doc/README.md) also tells the
three onboarding levels. Other READMEs link to it instead of retelling them. Each module's
`modules/<slug>/README.md` lists its commands in a table with the `$<id>` prefix, its requirements
and a Tests section. The template README covers clone, `./setup.sh`, `./run.sh`, the first-boot
flow, and what is committed and what is generated.

## Directory READMEs and comments

A folder gets a `README.md` when a newcomer would otherwise have to read several files to learn
what it is for. It says what the folder turns into what, which files to open first, and the
command that runs or tests it, usually in 20 lines. A change that alters what a folder does or
how it runs updates its README in the same change.

Code carries almost no comments. A one-line comment goes only where the code does something
surprising and renaming cannot remove the surprise, such as an invariant, a limit, a hardware fact
or a stored-data compatibility reason. Example code (core examples, the template's sample
modules) is the exception, because it explains the API to its reader.

## No process in code or docs

Code comments, READMEs and reference pages describe what is there now and why. They never carry
the process that produced them: no tool or session names, dates, decision or finding ids, run or
wave names, "was ..." or "used to" history, review or verification status, and no version tags on
features ("since 2.1"). Source files keep their SPDX and path header lines. History lives in git.

## The xewe workspace

Every repository carries the xewe workspace, which holds what the code does not
show. Its layout and rules are in [dot_agents](https://github.com/maxdokukin/dot_agents). READMEs
and `doc/` are for people and never point into it.

## Where documentation belongs

| Kind | Where |
|---|---|
| How to use or build this repository | its root `README.md` |
| What one folder does and how to run or test it | that folder's `README.md` |
| Rules for everything in the organization | `.github/guidelines/` |
| Rules a tool enforces | with that tool or the repository it checks (for example `xewe-os-core/doc/LIBRARY_RULES.md`, checked by `arduino-lint`) |
| Facts the code does not show | the repository's xewe workspace |
| A human guide too long for the README that belongs to no single folder | a `doc/` folder in the repository it belongs to, linked from that README |

Don't park documentation about one repository in another. Firmware docs that describe modules or
libraries belong in those repositories, with a link from the firmware README.
