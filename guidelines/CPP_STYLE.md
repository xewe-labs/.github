# C++ style

The firmware, libraries and modules share one style. Where this page and the formatter disagree,
the formatter wins.

## Formatting

`xewe format` formats C and C++ with clang-format and the XeWe style that ships inside
`xewe-os-tools`, and Python with `ruff format`. No repository commits a `.clang-format`.
`xewe format --check` is a blocking CI job in every repository.

The style is four spaces and no tabs, a 100-column limit, attached braces, `Type* name` and
`Type& name`, and nothing column-aligned (no aligned `#define`s, declarations, assignments or
trailing comments), so a one-line change diffs as one line. Include order is preserved, never
sorted.

## Files and names

* **XeWeCore is one library.** It has one file pair per component in `src/XeWeCore/`, named like
  the component (`Serial.h`/`Serial.cpp`, `Cli`, `Nvs`, `FlexData.h`, `Module`, `XeWeOs`), and
  header-only helpers in `src/XeWeCore/Utils/` behind `Utils.h`.
* **A module is one folder per class**, named exactly like the class, holding files with the same
  name: `src/Wifi/Wifi.h`, `src/Wifi/Wifi.cpp`.
* **Classes carry no prefix.** Everything in XeWeCore lives in `namespace xewe` (plus `xewe::str`
  and `xewe::color`). The only global XeWeCore name is `XeWeOs`, an alias of `xewe::Os`. Modules
  live in the global namespace.
* **Sketches and modules include `<XeWeCore.h>`**, the library's single top-level header. The
  reason is in
  [`LIBRARY_RULES.md`](https://github.com/xewe-labs/xewe-os-core/blob/main/doc/LIBRARY_RULES.md).
  Files inside a repository use relative quotes, and a module includes a required module the same
  way (`#include "../Wifi/Wifi.h"`), since installed modules sit side by side.
* **Headers use `#pragma once`**, after the license header.

## Names to avoid

The Arduino cores define `cli`, `sei`, `constrain`, `radians`, `degrees`, `sq`, `bit`, `lowByte`
and `highByte` as function-like macros, and the lowercase name directly followed by `(` expands.
So `xewe::Cli cli(serial);` does not compile, a standalone `xewe::Cli` object is named `xewe_cli`,
and `cli(` is never written anywhere.

## Debug output

Every class that prints debug output gets a flag that defaults to off and is switched on from the
build, never by editing the file:

```cpp
#ifndef DEBUG_Wifi
#define DEBUG_Wifi 0
#endif
```

Pass it as a compiler flag (`-DDEBUG_Wifi=1`). How a module ships the switch in its `Config.h`
is in the
[module contract](https://github.com/xewe-labs/xewe-os-modules/blob/main/doc/CONTRACT.md).

## Dependencies

* XeWeCore's standalone components (Utils, Serial, Cli, FlexData, Nvs) never include `Module.h`
  or `XeWeOs.h`. The include direction is Utils, then Serial, then Cli and FlexData, then Nvs,
  and all of them before Module and XeWeOs.
* Every library a repository includes is declared: libraries in `library.properties` under
  `depends=`, modules in `module.properties` under `depends_libraries` and `depends_modules`.
* The constructor rules for modules (`host`, `<var>_ref`, `[this]` captures) are in the
  [module contract](https://github.com/xewe-labs/xewe-os-modules/blob/main/doc/CONTRACT.md).

## Portability

XeWeCore declares `architectures=esp32` only, so these rules bind a library only if it declares
more. They exist because the ESP32 core builds at `-std=gnu++2b` with `-fexceptions`, while most
Arduino cores are C++17 with `-fno-exceptions`.

* **C++17 is the floor.** No `std::span`, no concepts, no C++20 library additions. Use
  `xewe::span` from XeWeCore.
* **No exceptions.** No `try`/`catch`, and no `std::stoi`/`stoll`/`stod`, because they throw.
  Parse with `xewe::str::parse_int` and `parse_float`, which return `false` on failure.
* **Platform-specific API goes behind a feature macro**, `XEWE_<AREA>_HAS_<FEATURE>`, declared
  with `#ifndef`, defaulted from core detection and overridable from build flags, in the same
  shape as the `DEBUG_<Class>` flags.
* **Optional vendor headers are gated with `__has_include`**, never a core-name `#ifdef`, and
  never from a header that a library's entry header includes unconditionally.
* **Avoid `Serial.printf`.** It is not on the `Print` class for AVR, SAMD or STM32. Format with
  `xewe::str::format` and print the result.
