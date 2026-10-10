# C++ style

The firmware, libraries and modules share one style. Where this page and the formatter disagree,
the formatter wins.

## Formatting

None of the repositories carries a formatter configuration yet (`xewe-os-tools` `SPEC.md`, open
point O10); until one does, match the rules below by hand.

The style: four spaces and no tabs, no column
limit (a signature or a comment banner stays on one line), attached braces, `Type* name` and
`Type& name`, case labels indented under their `switch`, one constructor initializer per line with
leading commas, and aligned columns for consecutive `#define`s, declarations and assignments.
Include order is preserved, never sorted.

## Files and names

* **XeWeCore is one library:** one file pair per component in `src/XeWeCore/`, named like the
  component (`Serial.h`/`Serial.cpp`, `Cli`, `Nvs`, `FlexData.h`, `Module`, `XeWeOs`), and the
  header-only helpers in `src/XeWeCore/Utils/` behind `Utils.h`.
* **A module is one folder per class**, named exactly like the class, holding files with the same
  name: `src/Wifi/Wifi.h`, `src/Wifi/Wifi.cpp`.
* **Classes carry no prefix.** Everything in XeWeCore lives in `namespace xewe` (plus `xewe::str`
  and `xewe::color`; there is no `xewe::os` namespace). The only global XeWeCore name is `XeWeOs`,
  an alias of `xewe::Os`. Modules live in the global namespace.
* **XeWeCore has exactly one top-level header**, `src/XeWeCore.h`, the umbrella that includes the
  sub-headers in `src/XeWeCore/`. Arduino puts every library's `src/` on the include path, so a
  second top-level header with a generic name collides with other libraries or with system headers.
* **Includes:** sketches and modules include `#include <XeWeCore.h>`. arduino-cli discovers a
  library only from its top-level headers, so a sketch whose only include is `<XeWeCore/Serial.h>`
  does not build; a `XeWeCore/<Part>.h` sub-header may be included after the umbrella. Files within
  a repository use relative quotes (`#include "Serial.h"` inside `src/XeWeCore/`); a module includes
  a required module the same way (`#include "../Wifi/Wifi.h"`), since installed modules sit side by
  side.
* **Headers use `#pragma once`**, after the license header.

## Names to avoid

The AVR and ESP32 Arduino cores define these as function-like macros: `cli`, `sei`, `constrain`,
`radians`, `degrees`, `sq`, `bit`, `lowByte`, `highByte`. Only the lowercase name directly
followed by `(` expands, so `xewe::Cli cli(serial);` does not compile while `obj.cli.loop()` and
`cli{serial}` are fine. That is why a standalone `xewe::Cli` object is named `xewe_cli` (as in
XeWeCore's `03_Cli` example), and why the `XeWeOs` member `os.cli` is brace-initialised and only
ever used as `os.cli.execute(...)`, never followed by `(`. Never write `cli(` anywhere.

## Debug output

Every class that prints debug output gets a flag that defaults to off and is switched on from the
build, never by editing the file:

```cpp
#ifndef DEBUG_Wifi
#define DEBUG_Wifi 0
#endif
```

Pass it as a compiler flag (`-DDEBUG_Wifi=1`, arduino-cli
`--build-property "compiler.cpp.extra_flags=-DDEBUG_Wifi=1"`). `xewe build --define` is not a
substitute: its values land in the generated `<XeWeBuildInfo.h>`, which the sketch's `Config.h`
includes, so they never reach a module's `.cpp` files. XeWeCore reads that header only for
`XEWE_TESTING` and `XEWE_DEVICE_NAME`.

## Dependencies

* XeWeCore's standalone components (Utils, Serial, Cli, FlexData, Nvs) never include `Module.h`
  or `XeWeOs.h`; the include direction is Utils ← Serial ← Cli, FlexData ← Nvs, all ← Module ←
  XeWeOs.
* Every library a repository includes is declared: libraries in `library.properties` → `depends=`,
  modules in `module.properties` → `depends_libraries` and `depends_modules`.
* Modules name the constructor's Os parameter `host` (a parameter named `os` hides the
  `Module::os` member), take the modules they need by reference as `<var>_ref`, register the
  requirement, and capture `[this]` only in handlers; see [`modules.md`](modules.md).

## Portability

XeWeCore declares `architectures=esp32` only (C3, C6, S3), so these rules bind a library only if
it ever declares more; XeWeCore's host tests still build with `-fno-exceptions`. Libraries that
declare architectures beyond `esp32` follow four rules. They exist because the
ESP32 core is unusually permissive: it builds at `-std=gnu++2b` with `-fexceptions`, while most
Arduino cores are C++17 with `-fno-exceptions`.

* **C++17 is the floor.** No `std::span`, no concepts, no C++20 library additions. Use
  `xewe::span` from XeWeCore.
* **No exceptions.** No `try`/`catch`, no `std::stoi`/`stoll`/`stod` — they throw. Parse with
  `xewe::str::parse_int` / `parse_float`, which report failure by returning `false`.
* **Platform-specific API goes behind a named feature macro**, `XEWE_<AREA>_HAS_<FEATURE>`,
  declared with `#ifndef`, defaulted from core detection, and overridable from build flags — the
  same shape as the `DEBUG_<Class>` flags (for example `XEWE_SERIAL_HAS_BUFFER_SIZING`);
  esp32-only XeWeCore needs none.
* **Optional vendor headers are gated with `__has_include`**, never a core-name `#ifdef`, and
  never from a header a library's entry header includes unconditionally.

Also avoid `Serial.printf`: it is not on the `Print` class for AVR, SAMD or STM32. Format with
`xewe::str::format` and print the result.

## What "done" means

Code compiles for ESP32-C3, C6 and S3 before it is committed: XeWeCore through its examples
(`publish.py check`) and its host tests (`tests/unit/run.sh`), a module through `xewe test --module <slug>
--all-chips` in an `xewe-os` harness, firmware through `xewe build --all-chips`, all with 0
warnings. A library that declares architectures beyond `esp32` also passes the host portability
check (`publish.py check`: C++17, `-fno-exceptions`, `-fno-rtti`, against a minimal Arduino shim),
which is the only automated evidence behind a non-ESP32 claim.

## Comments

A comment says what the code does or why: an invariant, a limit, a hardware or library fact, a
stored-data compatibility reason. No process in code (agent or session names, dates, decision or
finding ids, "was …" history); see [`documentation.md`](documentation.md#no-process-in-code-or-docs).
Example code (core examples, the template's example modules) explains the API to its reader.
