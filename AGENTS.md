# AGENTS.md — xewe-labs

Rules for coding agents working in any xewe-labs repository. A repository's own `AGENTS.md` or
`CLAUDE.md` describes that repository and wins where the two disagree.

## Never do these without being asked

* **Publish anything.** No `git push`, no tags, no GitHub releases, no `gh repo create`, no pull
  requests, no registry pull requests, no PlatformIO or Arduino Library Manager registration.
  Tags and registry entries are public and effectively permanent: a published library version can
  never be reused, and a registered library name can never change.
* **Commit.** Leave changes in the working tree and say what you changed. Commit only when asked,
  and never to `main` when the repository has a branch workflow.
* **Flash a board.** Compiling is fine; `-p <port>` uploads and also bumps the version counter.
* **Rewrite history**, force-push, or run git in a repository you were not asked to touch.

## Before changing code

* **Read the repository's README and its `CLAUDE.md`/`AGENTS.md` first.** They describe the layout
  and the invariants that are easy to break.
* **Find the source of truth.** Much of this organization is generated or copied: firmware modules
  are installed into `src/modules/`, the build toolchain is copied into `build/`, `library.json` is
  generated from `library.properties`, `Modules.h` and `Config.h` are generated. Editing a copy is
  lost on the next setup or build. Fix it where it comes from, then reinstall.
* **Match the surrounding code.** The conventions are in
  [`guidelines/cpp-style.md`](guidelines/cpp-style.md); the formatter is
  `build/scripts/<platform>/format.sh`.

## While working

* **Shell scripts:** check with `bash -n`. PowerShell cannot be run here, so mirror bash changes
  into the `.ps1` files carefully and say they are unverified.
* **Three platforms:** a change to a build script usually means changing the mac, linux and windows
  versions. A new `build_config` key means all three setup scripts.
* **Generated files:** when a change alters what a script generates, delete and regenerate rather
  than hand-editing the output.
* **Destructive commands:** `rm -rf` on a path built from a variable needs `${VAR:?}`, and prefer
  staging in a temp folder and swapping the result in, so a failed run leaves the old state intact.

## Reporting back

* **Say what you actually ran.** If something is unverified — PowerShell, a board you do not have,
  a network call that did not happen — say so plainly instead of implying it passed.
* **Report failures with their output**, and do not describe work as complete when a step was
  skipped.
* **Flag rule changes.** If you introduce a convention that is not yet written down here, say so,
  so it can be added or rejected.
