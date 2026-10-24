# Decisions

Each line records one decision that a later change could undo by accident. Write it as a date,
the decision, and the reason, with the newest line last.

2026-10-24 The organization-wide agent rules moved from the root `AGENTS.md` into this skill, because the xewe workspace allows a root `AGENTS.md` only in dot_agents.
2026-10-24 The guidelines have uppercase names (`guidelines/GIT_AND_RELEASES.md` and the rest) under the xewe_doc naming rule. `guidelines/license-header.txt` keeps its name, because it is not Markdown.
2026-10-24 `guidelines/DOCUMENTATION.md` follows xewe_doc where the two would disagree, so humans and agents work from one layout.
2026-10-24 No web flasher on the profile for now. GitHub strips scripts and iframes from the profile page, so a flasher would have to be a GitHub Pages site the profile links to. Any such site stays independent of the releases: no Pages job triggered by a release, no binaries fetched from release assets at build time, so pages and releases can change on their own schedules. `xewe-pages.yml` in xewe-os-tools couples the two and is not to be wired up.
