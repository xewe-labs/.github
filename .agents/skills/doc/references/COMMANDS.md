# Commands

Run the tools as `build/tools/.venv/bin/python -m xewe ...` from a project root. The bare
`xewe` script breaks after the project is moved.

One board is attached at a time, and each chip costs about a minute to compile. Build the
selected chip while iterating, and run `--all-chips` (c3, c6, s3) before calling a change done.

Core and module changes are tested in a copy of the `xewe-os` template, the harness, set up from
local checkouts with `XEWE_TOOLS_SOURCE`, `XEWE_CORE_SOURCE` and `XEWE_MODULES_SOURCE`.
`CONTRIBUTING.md` has the setup line.
