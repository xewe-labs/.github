# No board

Machines running agents usually have no board. `xewe flash`, `xewe run` and `xewe test` compile
first, then print `compiled, not run: no board attached (<chip>, <bin>)` and exit 0. That line
is not an error, so grep for it. Hardware tests are reported as "compiled, not run", while
`test_compiles` still really builds. Exit 4 (or 1 for `xewe test`) appears only with
`--require-board`.

In a compile-only session set `XEWE_NO_BOARD=1`, so no port is listed, probed or opened.

Report "compiled, not run" as exactly that, never as "passed on a board". A network call or a
board you don't have is unverified, and the report says so.
