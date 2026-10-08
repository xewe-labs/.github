# Documentation

Documentation lives next to what it describes, in the repository that owns it. A rule that spans
repositories lives in `.github`; a rule about one tool lives with that tool.

Documentation is part of the change, not a follow-up: a pull request that changes behaviour
updates the README in the same pull request.

## Every repository's README

Written for someone who arrived from a search result and does not know the organization. First
paragraph: what this is, what hardware or project it is for, and a link to the piece it plugs into.

Then, in this order, only the sections that apply:

| Section | |
|---|---|
| Overview / Features | what it does, in a few bullets |
| Installation / Setup | the shortest path to a working result, as commands that can be pasted |
| Usage | the common commands, with real examples |
| Configuration | the files someone edits, and what happens if they do |
| Project structure | a small tree or table; mark what is generated or installed |
| Development | how to change it, how to test it |
| License | one line |

Conventions that keep the repositories readable together:

* **Commands in fenced blocks, one per line**, with the flags someone actually needs and a short
  comment where it is not obvious.
* **Tables for anything with a shape** — commands, paths, metadata keys, where pieces come from.
* **Say what is generated.** Mark generated and installed paths, and name the script that creates
  them, so nobody edits a file that gets replaced.
* **Link across repositories** with full GitHub URLs, since these READMEs are read on their own.
* **No placeholder sections.** An empty "Roadmap" is worse than no section.

Repository specifics: XeWeCore's README shows the one include (`<XeWeCore.h>`), a minimal sketch,
the `os.serial`/`os.cli`/`os.nvs`/`os.system` members and its dependency, with the full reference
in its `doc/`. Each module's `modules/<slug>/README.md` lists its commands in a table with the
`$<id>` prefix, its requirements and a Tests section (preconditions and the `xewe test --module`
line); the modules repo's own README says what a module is and how to add one. The tools README
lists the command surface; the template README covers clone, `./setup.sh`, `./run.sh` and what is
committed vs generated. Why the ecosystem is shaped the way it is goes in
`xewe-os/ARCHITECTURE.md`, not in every README.

## Agent files

A repository that an agent works in carries an `AGENTS.md` (or `CLAUDE.md` where the tooling reads
that name) at its root (XeWeCore keeps it at `doc/AGENTS.md`, linked from its README, so it ships
with the library reference), describing what the README does not: the invariants, which files are
generated, what breaks silently, and what must never be run. It is short and prescriptive, and it
does not repeat the README. Organization-wide agent rules are in
[`../AGENTS.md`](../AGENTS.md).

## Where documentation belongs

| Kind | Where |
|---|---|
| How to use or build this repository | its `README.md` |
| Rules for everything in the organization | `.github/guidelines/` |
| Rules a tool enforces | with that tool (for example `publish-arduino-library/docs/library-rules.md`) |
| Instructions for agents | `AGENTS.md` in each repository, plus `.github/AGENTS.md` |
| Anything longer than a README section | a `doc/` folder in the repository it belongs to, linked from that README |

Do not park documentation about one repository in another. Firmware docs that describe modules or
libraries belong in those repositories, with a link from the firmware README.
