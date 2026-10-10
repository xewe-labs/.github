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
lists the command surface; the template README covers clone, `./setup.sh` (and the module
selection made at first setup), `./run.sh` and the first-boot flow, the example modules, and what
is committed vs generated.

The onboarding story has three levels and every README that introduces XeWe OS tells it the same
way: level 1 is XeWeCore from the Arduino Library Manager (example `01_Hello`), level 2 adds one
module of your own in the sketch folder (example `02_MyModule`), level 3 is the `xewe-os` template
with ready-made modules and the `xewe` tools. XeWeCore's `doc/README.md` is the reference text;
others link to it. Why the ecosystem is shaped the way it is goes in
`xewe-os/ARCHITECTURE.md`, not in every README.

## Agent files

A repository that an agent works in carries its agent files in `.agents/`, the WAX agentic
workspace (installed with its `wax_init` skill; core, modules and the template have it):

* `.agents/AGENTS.md`: the WAX entry text, then a project section: how to work in the repository
  (setup, build, flash and test commands, the generated files never to edit, `XEWE_NO_BOARD`, local
  sources and the harness), what breaks silently, what must never be run.
* `.agents/RULES.md`: the WAX rules, then the project rules (`X-01` …). Project rules that come
  from these guidelines summarise them and link here; these guidelines are the source, and a
  difference is fixed in the repository's `RULES.md`.
* `PREFERENCES.md`, `handoffs/` and `skills/` as WAX ships them.

There is no root `AGENTS.md` next to `.agents/`. A repository without `.agents/` (the tools,
`publish-arduino-library`) keeps a root `AGENTS.md` or `CLAUDE.md`. Agent files are short and
prescriptive and do not repeat the README. Organization-wide agent rules are in
[`../AGENTS.md`](../AGENTS.md).

## No process in code or docs

Code comments, docstrings, READMEs and reference pages describe what is there now and why: an
invariant, a limit, a hardware or library fact, a stored-data compatibility reason. They never
carry the process that produced them: no agent or session names, dates, decision or finding ids,
run or wave names, "was …" or "used to" history, review, report or verification status, and no
version tags on features ("since 2.1"). Source files keep their SPDX and path header lines.
History lives in the workspace's own records and in git, not in the repositories' files.

## Where documentation belongs

| Kind | Where |
|---|---|
| How to use or build this repository | its `README.md` |
| Rules for everything in the organization | `.github/guidelines/` |
| Rules a tool enforces | with that tool (for example `publish-arduino-library/docs/library-rules.md`) |
| Instructions for agents | `.agents/AGENTS.md` and `.agents/RULES.md` in each repository (a root `AGENTS.md` where there is no `.agents/`), plus `.github/AGENTS.md` |
| Anything longer than a README section | a `doc/` folder in the repository it belongs to, linked from that README |

Do not park documentation about one repository in another. Firmware docs that describe modules or
libraries belong in those repositories, with a link from the firmware README.
