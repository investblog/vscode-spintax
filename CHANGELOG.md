# Changelog

All notable changes to the Spintax VS Code extension are documented here.
This project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## 1.1.1 — 2026-08-09

Parity fixes against the engine, prompted by a rule-by-rule comparison with
Spintax Studio's tokenizer (`SpxTokens.pas`), whose every rule is measured
against the engine rather than read off a grammar.

### Fixed

- A `<X>` before the closing `]` of a permutation is no longer painted as a
  trailing separator. The engine extracts a trailing separator for every part
  but the last, so that `<X>` belongs to the last element and stays text.
- A trailing-separator candidate may no longer contain a top-level `|` or an
  unbalanced bracket. The engine splits a permutation into parts before looking
  for separators, so the `|` in `[a< | >|b]` is a part boundary (three options,
  no separator) and the `]` in `[A<]>|B]` really closes the permutation — the
  old pattern swallowed both. One level of balanced `{…}` or `[…]` inside the
  separator is still recognised (`<{a|b}>` is a literal separator to the
  engine, rendered verbatim); deeper nesting loses the highlight — a missing
  colour, never a wrong one.
- The trailing separator's HTML exclusions now apply to the trimmed inner text,
  as the engine's do: `< /b>`, `<b/ >` and `< li x>` are no longer painted as
  separators, while `<li >` and `< >` (blank separator) now are — exactly what
  the engine renders.
- Permutation config is now recognised with blanks after the `[`:
  `[ <sep=", ">a|b]` — the engine left-trims the body before asking whether it
  opens with `<` (the blanks themselves stay outside the config scope).
- Config keys are matched case-insensitively (`<SEP=", ">` is the key form, as
  the engine lowercases the config string before looking for keys).
- The config body is quote-aware end to end and refuses brackets: a quoted `>`
  no longer ends the config (`<"a>b";sep=1>` is one config, as the engine finds
  the first unquoted `>`), while a `]` anywhere in the candidate — quoted or
  not — stops it from being one, because the engine's bracket matching is a
  plain counter and that `]` has already closed the permutation
  (`[<sep=x]y>done]`). A config with no closing `>` on its line no longer
  bleeds its highlight across the rest of the document.
- An HTML start tag carrying a key-shaped attribute is content, not config:
  `[<li data-sep=1>x</li>|b]` keeps its tag plain, because the engine checks
  for an HTML tag before it looks for config keys.
- A directive preceded by closed inline comments on the same line is now
  highlighted: `/# c #/#set %x% = 1` really is a directive, because the engine
  strips comments before extracting directives. Text between comments still
  unmakes it, and a comment left open from an earlier line stays a known,
  deliberate under-claim.

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

### Fixed

- Directives on their own line inside a multi-line `{ … }` or `[ … ]` are now
  highlighted. The engine extracts `#set`/`#def` from the whole source
  line-by-line, independent of bracket nesting, so such a line really does define
  a variable — it was previously rendered as plain enumeration text. Pre-existing
  for `#set`; fixed for both.

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
