# Modules

A module is one feature (WiFi, a schedule, a relay) that someone figured out once so nobody has to
figure it out again. All modules live in one repository,
[`xewe-os-modules`](https://github.com/xewe-labs/xewe-os-modules), under `modules/<slug>/`.

**The rules are in
[`xewe-os-modules/CONTRACT.md`](https://github.com/xewe-labs/xewe-os-modules/blob/main/CONTRACT.md)**,
with its project rules in
[`.agents/RULES.md`](https://github.com/xewe-labs/xewe-os-modules/blob/main/.agents/RULES.md) and the
working steps in [`.agents/AGENTS.md`](https://github.com/xewe-labs/xewe-os-modules/blob/main/.agents/AGENTS.md).
This page only lists what is never negotiable; where they differ, `CONTRACT.md` wins.

## Non-negotiables

1. **One repo.** A module is a folder `modules/<slug>/` in `xewe-os-modules` and arrives by pull
   request. There are no per-module repositories, sketches or build scripts.
2. **Layout.** `module.properties`, `src/<Folder>/<Folder>.h`, `src/<Folder>/<Folder>.cpp`,
   `tests/board/test_<slug>.py`, optional `tests/unit/`, and `README.md`; nothing else. Only
   `src/<Folder>/` is installed into a firmware (`build/modules/src/<Folder>/`); the tests are
   copied to `build/modules/tests/<slug>/`.
3. **`module.properties`** has every key, in the contract's order, even when empty, including
   `requires_core` in comma form (`>=2.1.0,<3.0.0`; 2.1.0 is the floor for the settings table,
   listeners and pin claims). Third-party libraries in `depends_libraries` are pinned in the
   repository's `libraries.toml`.
4. **`id` is at most 15 characters** (`[a-z][a-z0-9_]*`). It is the CLI group (`$<id>`) and the
   NVS namespace, so it never changes once released. `slug` and `folder` are fixed too.
5. **The declare line stays explicit:** `declare=<Folder> <var>(os[, <dep var>...]);` is copied
   verbatim into the generated `Modules.h`. The type is the folder, the first argument is `os`, the
   other arguments are the variables of modules listed in `depends_modules`.
6. **Code.** `class <Folder> : public xewe::Module` in the global namespace. The header includes
   `<XeWeCore.h>` and no other XeWeCore header; a required module is included relatively
   (`#include "../Wifi/Wifi.h"`).
7. **The constructor's Os parameter is named `host`**, dependency parameters `<var>_ref`. Bodies
   and lambdas use the member `os`, never `this->os`.
8. **Handlers capture `[this]` only**, never `[&]` or `[=]`.
9. **Never write `cli(`.** arduino-esp32 defines `cli()` as a macro: use `os.cli.execute("...")`
   and brace-initialise anything named `cli`.
10. **Three required tests** in `tests/board/test_<slug>.py`: `test_compiles` (really builds, even with
    no board), `test_status` (`$<id> status` prints `<name> module (enabled|disabled)`) and one
    behaviour test on one of the module's own commands.
11. **The validator must pass:** `tools/validate.py` exits 0 before a change is done.

## Before a module change is done

Run from a **copy** of the `xewe-os` template used as the harness:

```bash
XEWE_MODULES_SOURCE=<modules repo> ./setup.sh --modules <slug>                  # in the harness
<harness>/build/tools/.venv/bin/python tools/validate.py --harness <harness>   # in the modules repo
build/tools/.venv/bin/python -m xewe test --module <slug> --all-chips          # in the harness: c3, c6, s3
```

With `XEWE_MODULES_SOURCE` the harness uses the working tree directly; run `xewe modules generate`
in the harness after each edit. Without a board, `test_compiles` passes or fails for real and the
serial tests report "compiled, not run". The full gate (each module alone and all modules
together) is in the modules repository's `.agents/AGENTS.md`.

## Changing a released module

Removing or renaming a command changes the interface people script against: bump the module's
`version` MINOR for additions and MAJOR for breaking changes (pre-1.0: MINOR for breaking), and say
so in its README. The repo is tagged as a whole (`vX.Y.Z`; until the ecosystem's `v3.0.0` tag set,
projects follow `latest`), and projects move to a tag with `xewe manifest update`.
