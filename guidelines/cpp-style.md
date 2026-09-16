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

The ESP32 Arduino core defines these as function-like macros: `cli`, `sei`, `constrain`,
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

## What "done" means

Code compiles for ESP32-C3, C6 and S3 before it is committed — a library through its examples, a
module through its `scripts/validate.sh`, firmware through `build.sh`.
