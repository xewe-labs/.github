<!-- One topic per pull request. A change across repositories gets one pull request per
repository, each linking the others. -->

**What and why**

**Checklist**
- [ ] Builds for all three chips with 0 warnings: `build/tools/.venv/bin/python -m xewe build --all-chips`
      (core: `tests/unit/run.sh` and the examples; modules: `xewe test --module <slug> --all-chips` in a harness)
- [ ] `xewe format --check` and `xewe brand-lint` pass, and CI is green
- [ ] Modules: `tools/validate.py --harness <harness>` exits 0, and `doc/modules.md` is regenerated if metadata changed
- [ ] Tests added or updated (a module has `test_compiles`, `test_status` and one behaviour test;
      core logic gets a host test in `tests/unit/`; the tools get pytest cases)
- [ ] No generated files committed: nothing from `build/`, `src/Modules.h`, `run.sh`, `.venv/` or
      caches
- [ ] README, `doc/` and the xewe workspace (modules: `doc/CONTRACT.md`) updated in this pull request where behaviour changed
- [ ] Versions: no hand bump, because versions change only through `scripts/release.sh`; `xewe.toml`
      refs untouched unless this pull request moves one on purpose

**What I tested**
<!-- Commands run and boards used. "compiled, not run" (no board) is fine; say so. Name what you
did not test. -->
