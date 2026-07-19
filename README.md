```
███╗   ██╗██████╗ ███╗   ███╗
████╗  ██║██╔══██╗████╗ ████║
██╔██╗ ██║██████╔╝██╔████╔██║
██║╚██╗██║██████╔╝██║╚██╔╝██║
██║ ╚████║██║     ██║ ╚═╝ ██║
╚═╝  ╚═══╝╚═╝     ╚═╝     ╚═╝
```

[![arb](https://img.shields.io/badge/arb-package-05d9e8?style=flat-square)](https://github.com/MenkeTechnologies/arb)
![type](https://img.shields.io/badge/self--sourcing-dashboard-9b5de5?style=flat-square)
![license](https://img.shields.io/badge/license-MIT-ff2a6d?style=flat-square)

### `WATCH YOUR NPM DEPENDENCIES DRIFT OUT OF DATE, LIVE`

> *"Every stale package, current-vs-wanted-vs-latest, refreshed on a ten-second heartbeat."*

`npm` is a self-sourcing terminal dashboard that lists the outdated dependencies in the current project. It runs `npm outdated` on its own timer, so nothing has to be piped into it — open it and the table stays live. It is an `arb` package: a dashboard for [`arb`](https://github.com/MenkeTechnologies/arb), the pipeline-to-TUI language on the fusevm bytecode VM.

### [`arb`](https://github.com/MenkeTechnologies/arb) &middot; [`arb-registry`](https://github.com/MenkeTechnologies/arb-registry)

---

## Table of Contents

- [\[0x00\] Overview](#0x00-overview)
- [\[0x01\] Install](#0x01-install)
- [\[0x02\] Usage](#0x02-usage)
- [\[0x03\] The Spec](#0x03-the-spec)
- [\[0x04\] How It Works](#0x04-how-it-works)
- [\[0xFF\] License](#0xff-license)

## [0x00] OVERVIEW

`npm` surfaces the outdated npm dependencies in whatever project you launch it from. Instead of running `npm outdated` by hand and re-reading a static wall of text, the dashboard re-runs it on a fixed interval and renders the result as a live list, so the current, wanted, and latest columns stay fresh while you work.

## [0x01] INSTALL

```sh
arb install npm      # install from the arb-registry git index
arb search npm       # find it in the registry
```

## [0x02] USAGE

```sh
arb npm                     # run it (self-sourcing — no pipe needed)
arb npm --serve             # browser dashboard
arb npm --html > npm.html   # emit a static HTML dashboard
```

## [0x03] THE SPEC

```arb
# npm — Outdated npm dependencies in the current project, self-sourced from `npm outdated`.
! npm outdated every 10s
list .npm
source .npm { in }
grid .npm -row 0 -col 0
```

- `! npm outdated every 10s` is the self-source: arb runs `npm outdated` every ten seconds and feeds its stdout into the `.npm` stream, so the dashboard supplies its own data with nothing piped in.
- `list .npm` declares a list widget bound to the `.npm` stream.
- `source .npm { in }` is the query pipeline for the stream; `in` passes the incoming lines straight through to the widget unchanged.
- `grid .npm -row 0 -col 0` places the `.npm` widget at row 0, column 0 of the dashboard grid.

## [0x04] HOW IT WORKS

The `!` self-source fires `npm outdated` on the ten-second timer and pushes its stdout into the `.npm` stream. The `source .npm { in }` pipeline shapes that stream — here it forwards the rows as-is — and the `list` widget renders them at its grid slot. arb lowers the pipeline compute to the fusevm bytecode VM with Cranelift JIT, so each refresh is compiled and executed rather than re-parsed.

## [0xFF] LICENSE

MIT.
