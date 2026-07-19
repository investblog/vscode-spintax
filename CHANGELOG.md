# Changelog

All notable changes to the Spintax VS Code extension are documented here.
This project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## 1.1.0 — 2026-07-19

### Added

- Highlighting for the `#def` directive, shipped in Spintax engine 3.0.0 /
  `@spintax/core` 0.3.0. It shares `#set`'s line-anchored shape
  (`#def %name% = value`) but different semantics: `#set` is a macro whose value is
  re-substituted and re-rolled at every reference, while `#def` resolves once per
  render and holds that result everywhere.
- `#def` gets its own scope, `keyword.control.directive.def.spintax`, rather than
  sharing `#set`'s. The two directives mean genuinely different things, so a theme
  must be able to colour them apart.

## 1.0.1 — 2026-07-12

### Changed

- Added an extension icon (`images/icon.png`) for the Marketplace listing.
- Replaced the retired shields.io Marketplace badges with `vsmarketplacebadges.dev`.
- Rewrote the constructs table as HTML so pipes in examples render correctly on the
  Marketplace (the Markdown renderer showed literal `\|` escapes).

## 1.0.0 — 2026-07-08

First release. Engine-accurate TextMate grammar for the full spintax surface, matching the
[`@spintax/core`](https://www.npmjs.com/package/@spintax/core) contract and verified headlessly
with `vscode-tmgrammar-test`.

### Added

- Enumerations `{a|b|c}`, permutations `[<config>a|b]`, variables `%name%`,
  `#set` / `#include` directives, comments `/# … #/`.
- **Conditionals** `{?VAR?then|else}` / `{?!VAR?…}` with a strict opener (ASCII identifier +
  mandatory `?`).
- **Plurals** `{plural N: form|…}` requiring the `{plural ` prefix and a mandatory `:`.
- **Engine-accurate permutation separators**: known-key config vs HTML content
  (`[<li>a</li>|b]`, `[<a href="/x">…</a>|b]` stay content); leading `<and>` and per-element
  trailing separators (`[a<, >|b]`, `[a<and>|b]`) are highlighted — mirroring the engine's
  `looksLikeHtmlStartTag` / `extractTrailingSep` asymmetry.
- Block-comment toggle (`/# … #/`) and auto-closing of `{}`, `[]`, `%%`, `""`.
