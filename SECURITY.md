# Security

## Reporting a vulnerability

Report suspected vulnerabilities through
[GitHub's private vulnerability reporting](https://github.com/xewe-labs) on the affected
repository (Security → Report a vulnerability). Do not open a public issue for something
exploitable.

Include what the problem is, which repository and version, and how to reproduce it.

## Scope and expectations

These are hobby-scale firmware and tooling projects maintained by one person. There is no
guaranteed response time and no bug bounty. Fixes land on `main` and in the next release.

Assume a device running this firmware is only as safe as the network it sits on:

* The command line is unauthenticated. Anything that can reach the serial port or the HTTP
  endpoint can run every command.
* Credentials stored in NVS (for example WiFi) are not encrypted.
* Nothing is signed, and firmware updates are not verified.

Treat these as known properties of the design rather than vulnerabilities. Reports that change
them — a way to authenticate the HTTP interface, for example — are welcome as feature proposals.
