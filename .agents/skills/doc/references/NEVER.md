# Never without being asked

These hold in every xewe-labs repository unless the human asks for them in this session.

- Run `scripts/release.sh`. It commits, tags, pushes and publishes, and a human runs it. Core's
  script has `--dry-run`, which is safe to run. `xewe release` builds the release matrix and only prints the git and gh
  commands. A tag or a registry entry is public and permanent, and a published library
  version can never be reused.
- Create, rename or archive a repository.
- Touch `~/.arduino15` or `~/Arduino`, or run `arduino-cli` directly. The tools keep their own
  toolchain in `~/.xewe-os/build-tools/`, shared by all projects (`XEWE_HOME` moves it), and use
  the project's `build/`. Reuse an installed esp32 core with `./setup.sh --arduino-data DIR`.
  Never delete the shared toolchain as a fix.
- Flash a board or require one. Don't pass `--require-board` or `--erase`, and don't run
  `./run.sh` or `xewe flash` to a real port. `xewe run` erases the flash by default. Compiling is
  always fine.
- Edit `xewe.toml`. Only `xewe manifest update`, `xewe modules select`, `xewe setup --modules`,
  the first-setup menu and `xewe release` write it, and only when asked.
- Run `rm -rf` on a path built from a variable without `${VAR:?}`. Prefer staging in a temp
  folder and swapping the result in.
