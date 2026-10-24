# Code rules that break silently

- The one library include is `#include <XeWeCore.h>`. arduino-cli discovers a library only from
  its top-level headers, so a sketch or module whose only XeWeCore include is
  `<XeWeCore/<Part>.h>` does not build. Sub-headers may follow the umbrella.
- Module constructors name the Os parameter `host`. The member is `os`, and a parameter named
  `os` hides it. Bodies use `os.serial`, `os.nvs` and `os.cli`, never `this->os`.
- Handlers capture `[this]` only, because `[&]` silently binds a shadowing parameter.
- Never write `cli(`. arduino-esp32 defines `cli()` as a macro. Use `os.cli.execute("...")`, and
  name a standalone `xewe::Cli` object `xewe_cli`.
- Fix a generated file where it comes from, then regenerate, because an edited copy is lost.
  In a project, `./setup.sh` regenerates `build/` (with `build/modules/` and `modules.lock`),
  `src/Modules.h` and `run.sh`. Every build regenerates `XeWeBuildInfo.h`. `scripts/release.sh`
  sets the version lines of a release, and `tools/validate.py --write-index` writes the modules'
  `doc/modules.md`.
- Comments describe the code, never the process that produced it. They carry no agent or
  session names, dates, decision ids or "was ..." history (`guidelines/DOCUMENTATION.md`, "No
  process in code or docs").
- Dotenv and key files are read by the tools, never by hand.

The C++ rules in full are in `guidelines/CPP_STYLE.md`. The module rules are in
`xewe-os-modules/doc/CONTRACT.md`.
