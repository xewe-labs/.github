# C++ style

The firmware, libraries and modules share one style. Where this page and the formatter disagree,
the formatter wins.

## Formatting

`clang-format` plus a few custom passes, configured in the build toolchain at
`tools/code_formatter/.clang-format` and run from a project that has the toolchain installed:

```bash
build/scripts/<mac|linux>/format.sh            # format <project>/src
build/scripts/<mac|linux>/format.sh --check    # report only
```

What it enforces, so hand-written code lands close to it: four spaces and no tabs, no column
limit (a signature or a comment banner stays on one line), attached braces, `Type* name` and
`Type& name`, case labels indented under their `switch`, one constructor initializer per line with
leading commas, and aligned columns for consecutive `#define`s, declarations and assignments.
Include order is preserved, never sorted.

## Files and names

* **One folder per class**, named exactly like the class, holding files with the same name:
  `src/AsyncTimer/AsyncTimer.h`, `src/AsyncTimer/AsyncTimer.cpp`.
* **Classes carry no prefix.** Library code lives in `namespace xewe`, the framework in
  `namespace xewe::os`; modules live in the global namespace.
* **A library has exactly one top-level header**, `src/XeWe<Name>.h`, which only includes the
  files in its subfolders. Arduino puts every library's `src/` on the include path, so a second
  top-level header with a generic name collides with other libraries or with system headers.
* **Includes:** other libraries only through their entry header (`#include <XeWeUtils.h>`,
  `#include <XeWeOS.h>`); files within a repository with relative quotes
  (`#include "../FlexData/FlexData.h"`); other installed modules the same way
  (`#include "../Wifi/Wifi.h"`), since modules sit side by side.
* **Headers use `#pragma once`**, after the license header.

## Names to avoid

The AVR and ESP32 Arduino cores define these as function-like macros: `cli`, `sei`, `constrain`,
`radians`, `degrees`, `sq`, `bit`, `lowByte`, `highByte`. Only the lowercase name directly
followed by `(` expands, so `xewe::Cli cli(serial);` does not compile while `obj.cli.loop()` and
`cli{serial}` are fine. Name such objects `xewe_cli`.

## Debug output

Every class that prints debug output gets a flag that defaults to off and is switched on from the
build, never by editing the file:

```cpp
#ifndef DEBUG_Wifi
#define DEBUG_Wifi 0
#endif
```

```bash
build/scripts/linux/build.sh -c c3 --build-property "compiler.cpp.extra_flags=-DDEBUG_Wifi=1"
```

## Dependencies

* A library works on its own and never includes `XeWeOS.h` from its standalone headers.
* Every library a repository includes is declared: libraries in `library.properties` → `depends=`,
  modules in `module.properties` → `depends_libraries` and `depends_modules`.
* Modules take the modules they need by reference in the constructor and register the requirement;
  see [`modules.md`](modules.md).

## Portability

Libraries that declare architectures beyond `esp32` follow four rules. They exist because the
ESP32 core is unusually permissive: it builds at `-std=gnu++2b` with `-fexceptions`, while most
Arduino cores are C++17 with `-fno-exceptions`.

* **C++17 is the floor.** No `std::span`, no concepts, no C++20 library additions. Use
  `xewe::span` from XeWeUtils.
* **No exceptions.** No `try`/`catch`, no `std::stoi`/`stoll`/`stod` — they throw. Parse with
  `xewe::str::parse_int` / `parse_float`, which report failure by returning `false`.
* **Platform-specific API goes behind a named feature macro**, `XEWE_<AREA>_HAS_<FEATURE>`,
  declared with `#ifndef`, defaulted from core detection, and overridable from build flags — the
  same shape as the `DEBUG_<Class>` flags. `XEWE_SERIAL_HAS_BUFFER_SIZING` is the example.
* **Optional vendor headers are gated with `__has_include`**, never a core-name `#ifdef`, and
  never from a header a library's entry header includes unconditionally. `XeWeUtils.h` includes
  `LockGuard/LockGuard.h` this way.

Also avoid `Serial.printf`: it is not on the `Print` class for AVR, SAMD or STM32. Format with
`xewe::str::format` and print the result.

## What "done" means

Code compiles for ESP32-C3, C6 and S3 before it is committed — a library through its examples, a
module through its `scripts/validate.sh`, firmware through `build.sh`. A library that declares
architectures beyond `esp32` also passes the host portability check (`publish.py check`: C++17,
`-fno-exceptions`, `-fno-rtti`, against a minimal Arduino shim), which is the only automated
evidence behind the non-ESP32 claim.
