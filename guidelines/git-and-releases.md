# Git and releases

## Branches

`main` is always buildable. Work on a branch named for the change (`fix-upload-timeout`,
`module-relay`), and merge through a pull request. Direct commits to `main` are for the
maintainer's own small changes only.

## Commits

One topic per commit. Subject in the imperative, lowercase, no trailing period, under 72
characters:

```
add scheduler module
fix wifi reconnect after deep sleep
remove esptool installation prompt
```

Add a body when the reason is not obvious from the diff: what was wrong, what changed, what it
affects. Two cases that always need one:

* **A change that breaks projects until they re-run setup** — a renamed `build_config` key, a moved
  file, a changed generated layout. Say so in the message.
* **A change made mostly by an agent.** Keep the `Co-Authored-By` trailer that records it.

Never commit generated or installed files: installed modules and toolchain, `build/libraries/`,
`builds/`, `.venv/`, `build_config`, `Modules.h`, `modules.lock`, `library.json` is the exception —
it is generated but committed, because registries read it from the repository.

## Versions

| | Where the version lives | Format |
|---|---|---|
| Library | `library.properties` → `version=`, mirrored into `library.json` | `X.Y.Z` |
| Module | `module.properties` → `version=` | `X.Y.Z` |
| Firmware | `build/version_state` (`MAJOR`, `MINOR`, `PATCH`, `BUILD_ID`) | `X.Y.Z` |
| Tooling | git tag only | `vX.Y.Z` |

Semantic versioning, read from the consumer's side: MAJOR when existing firmware, sketches or
scripts must change, MINOR for new capability that older callers ignore, PATCH for fixes.

Firmware `PATCH` is not hand-written — `build.sh` bumps it and `BUILD_ID` on every build and
injects the result into `Config.h`. Set `MAJOR` and `MINOR` by editing `build/version_state`.

Libraries are versioned in lockstep by the publishing tool, which also updates the `depends=`
constraints across the libraries. Do not bump a library version by hand.

## Tags and releases

| | Tag | Created by |
|---|---|---|
| Library | `X.Y.Z` | `publish-arduino-library` (`publish.py release --execute`) |
| Firmware | `v<version>` | the toolchain's `release.sh` |
| Tooling | `vX.Y.Z` | by hand, when a project needs to pin a version |

A tag is permanent: registries reject a reused version, and other projects pin to what you
published. Never move or delete a published tag; release a new patch version instead.

Firmware releases go through `build/scripts/<mac|linux>/release.sh`, which builds every row of
`release_matrix.csv`, writes the binaries and manifests into `static/firmware/releases/<version>/`
for the web flasher, tags the repository and creates the GitHub release. Release notes say what
changed for someone flashing the device, not what changed in the code.

Library releases go through `publish-arduino-library`, which checks the libraries, bumps versions,
tags, creates GitHub releases and — the first time — opens the Arduino Library Manager pull
request. Run it without `--execute` first and read the plan.
