# Ecosystem

These facts apply in every xewe-labs repository. A repository's own `.agents/skills/doc/`
describes that repository and wins where the two disagree. The organization rules themselves
are in `guidelines/`, and each repository's doc skill summarises the ones that apply to it and
links back. The commands in these references are the same ones a human runs.

| Repository | What it is | Its agent files |
|---|---|---|
| `xewe-os` | the firmware template users clone; holds only its own code and `xewe.toml` | [`.agents/skills/doc/`](https://github.com/xewe-labs/xewe-os/blob/main/.agents/skills/doc/SKILL.md) |
| `xewe-os-core` | the `XeWeCore` Arduino library (utils, serial, cli, nvs, `XeWeOs`, `xewe::Module`) | [`.agents/skills/doc/`](https://github.com/xewe-labs/xewe-os-core/blob/main/.agents/skills/doc/SKILL.md) |
| `xewe-os-modules` | every module, `modules/<slug>/`, plus `doc/CONTRACT.md` and `tools/validate.py` | [`.agents/skills/doc/`](https://github.com/xewe-labs/xewe-os-modules/blob/main/.agents/skills/doc/SKILL.md) |
| `xewe-os-tools` | `xewe`: setup, build, flash, serial, provision, test, boards, modules, manifest, format, brand-lint, release, webui; the reusable CI workflows | [`.agents/skills/doc/`](https://github.com/xewe-labs/xewe-os-tools/blob/main/.agents/skills/doc/SKILL.md) |
| `.github` | this repository: organization guidelines, profile, issue templates | this skill |

Why the ecosystem is shaped this way:
[`xewe-os/doc/XEWE_OS_ECOSYSTEM.md`](https://github.com/xewe-labs/xewe-os/blob/main/doc/XEWE_OS_ECOSYSTEM.md).
