# Modules

A module is one feature (WiFi, a schedule, a relay) that someone figured out once so nobody has to
figure it out again. All modules live in one repository,
[`xewe-os-modules`](https://github.com/xewe-labs/xewe-os-modules), under `modules/<slug>/`.

**The rules are in
[`xewe-os-modules/doc/CONTRACT.md`](https://github.com/xewe-labs/xewe-os-modules/blob/main/doc/CONTRACT.md)**.
This page lists only what is never negotiable, and where the two differ, `doc/CONTRACT.md` wins.

## Non-negotiables

1. **One repo.** A module is a folder `modules/<slug>/` in `xewe-os-modules` and arrives by pull
   request. There are no per-module repositories, sketches or build scripts.
2. **Layout.** `module.properties`, `src/<Folder>/<Folder>.h`, `src/<Folder>/<Folder>.cpp`,
   optional `src/<Folder>/Config.h`, `tests/board/test_<slug>.py`, optional `tests/unit/`,
   and `README.md`; nothing else. Only
   `src/<Folder>/` is installed into a firmware (`build/modules/src/<Folder>/`); the tests are
   copied to `build/modules/tests/<slug>/`.
3. **`module.properties`** has every key, in the contract's order, even when empty, including
   `requires_core` in comma form (`>=3.0.0,<4.0.0` for the 3.x modules). Third-party libraries
   in `depends_libraries` are pinned in the repository's `libraries.toml`.
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
serial tests report "compiled, not run". The full gate builds each module alone and all modules
together, which is what the modules repository's `scripts/release.sh` runs.

## Changing a released module

Removing or renaming a command changes the interface people script against. Release an addition
as a MINOR version and a breaking change as MAJOR, and say so in the module's README and its
release notes. Each module is released on its own as tag `<module>/X.Y.Z` by
`scripts/release.sh X.Y.Z <module>`, which also sets its `version`. `scripts/release.sh X.Y.Z --all`
releases every module and tags the set `X.Y.Z`, a commit where every module is released at
`X.Y.Z`. Projects pin the set tag or a module tag, and move to a new one with
`xewe manifest update`. The steps are in the modules repository's README ("Releasing") and
[`GIT_AND_RELEASES.md`](GIT_AND_RELEASES.md).
