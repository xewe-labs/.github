---
name: Bug report
about: Something in the firmware, XeWeCore, a module or the xewe tools does not work as documented
title: ""
labels: bug
---

**What happened**
<!-- One or two sentences. Paste the real output (serial console, `xewe` command), not a summary. -->

**What you expected**

**How to reproduce**
<!-- Commands from a fresh project, one per line. For example:
./setup.sh --modules wifi,time
build/.venv/bin/python -m xewe build --chip c3
build/.venv/bin/python -m xewe serial --send '$time status'
-->

**Versions**
<!-- Paste `build/.venv/bin/python -m xewe lock show` (core, modules and tools refs) and the
`[project] version` from xewe.lock. -->

**Board**
- Chip: <!-- c3 / c6 / s3, or "no board (compile only)" -->
- Board / module: <!-- e.g. ESP32-C3 SuperMini -->
- Host OS and arch: <!-- e.g. Linux arm64, macOS x86_64 -->

**Modules selected**
<!-- `build/.venv/bin/python -m xewe modules list` (the `*` rows). -->

**Anything else**
<!-- Wiring, first-boot answers, whether NVS was erased. Do not paste Wi-Fi credentials. -->
