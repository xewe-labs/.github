<!-- One topic per pull request. A change across repositories gets one pull request per
repository, each linking the others. -->

**What and why**

**Checklist**
- [ ] Builds for all three chips with 0 warnings: `build/tools/.venv/bin/python -m xewe build --all-chips`
      (core: `publish.py check xewe-os-core`; modules: `xewe test --module <slug> --all-chips` in a harness)
- [ ] Modules: `tools/validate.py --harness <harness>` exits 0, and `MODULES.md` is regenerated if metadata changed
- [ ] Tests added or updated (a module has `test_compiles`, `test_status` and one behaviour test;
      core logic gets a host test in `tests/unit/`; the tools get pytest cases)
- [ ] No generated files committed: nothing from `build/`, `src/Modules.h`, `.venv/` or
      caches
- [ ] README / `.agents/` / `CONTRACT.md` updated in this pull request where behaviour changed
- [ ] Versions: no hand bump of XeWeCore; module `version` bumped for command changes; `xewe.toml`
      refs untouched unless this pull request moves one on purpose

**What I tested**
<!-- Commands run and boards used. "compiled, not run" (no board) is fine; say so. Name what you
did not test. -->
