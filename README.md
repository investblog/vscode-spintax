# Spintax for VS Code

[![Version](https://vsmarketplacebadges.dev/version-short/301st.spintax.svg?label=Marketplace&color=blue)](https://marketplace.visualstudio.com/items?itemName=301st.spintax)
[![Installs](https://vsmarketplacebadges.dev/installs-short/301st.spintax.svg?color=blue)](https://marketplace.visualstudio.com/items?itemName=301st.spintax)
[![CI](https://github.com/investblog/vscode-spintax/actions/workflows/ci.yml/badge.svg)](https://github.com/investblog/vscode-spintax/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)
[![Spintax Studio](https://img.shields.io/badge/Spintax_Studio-Microsoft_Store-0078D4)](https://apps.microsoft.com/detail/9mw3ch7b530p)

Syntax highlighting for [**spintax**](https://spintax.net) templates in Visual Studio Code —
engine-accurate against the [`@spintax/core`](https://www.npmjs.com/package/@spintax/core)
contract and verified headlessly with `vscode-tmgrammar-test`. Applies to `.spintax` and
`.gtw` files.

> Prefer a dedicated workspace? [**Spintax Studio**](https://apps.microsoft.com/detail/9mw3ch7b530p)
> is a native desktop editor for spintax — source and live preview side by side, validation
> with precise diagnostics, variant generation and export — built on the same engine this
> grammar is measured against ([source](https://github.com/investblog/spintax-studio)).

<table>
<thead><tr><th>Construct</th><th>Example</th></tr></thead>
<tbody>
<tr><td>Enumeration</td><td><code>{a|b|c}</code></td></tr>
<tr><td>Permutation</td><td><code>[&lt;minsize=2;maxsize=3;sep=", "&gt;a|b|c]</code></td></tr>
<tr><td>Variable</td><td><code>%name%</code></td></tr>
<tr><td>Local set</td><td><code>#set %name% = value</code> &mdash; macro, re-rolled at every reference</td></tr>
<tr><td>Local def</td><td><code>#def %name% = value</code> &mdash; resolved once per render, held everywhere</td></tr>
<tr><td>Include</td><td><code>#include "slug-or-id"</code></td></tr>
<tr><td>Conditional</td><td><code>{?VAR?then|else}</code> · <code>{?!VAR?…}</code></td></tr>
<tr><td>Plural</td><td><code>{plural %n%: one|few|many}</code></td></tr>
<tr><td>Comment</td><td><code>/# … #/</code></td></tr>
</tbody>
</table>

## Example

```spintax
/# hero block #/
#set %product% = Acme
#def %tone% = {friendly|warm}
{Welcome to|Meet} %product% — %tagline%, trusted since {2019|2020}.
Ships with [<minsize=2;maxsize=3;sep=", ";lastsep=" and ">SSO|audit logs|alerts]{?free? — free tier available|}.
You have %n% {plural %n%: message|messages}.
```

## Install

- **Marketplace:** open the Extensions view (<kbd>Ctrl/Cmd</kbd>+<kbd>Shift</kbd>+<kbd>X</kbd>),
  search **Spintax**, and install — or run `code --install-extension 301st.spintax`.
- **From VSIX:** download the `.vsix` from the [latest release](https://github.com/investblog/vscode-spintax/releases/latest)
  and run **Extensions: Install from VSIX…**.

## Features

- Full, engine-accurate tokenization of every spintax construct, including nested spintax
  inside conditional branches and enumerations.
- **Correct permutation config:** `<minsize=…;sep=…>` is config — case-insensitive keys,
  quote-aware values (`<sep="a>b">` is one config), blanks allowed after the `[` — while
  HTML inside items (`[<li>a</li>|b]`, `[<a href="/x">…</a>|b]`, even a key-shaped
  attribute like `[<li data-sep=1>…</li>|b]`) is content, not mis-highlighted as config.
  Genuine separators (`[<and>a|b]`, `[a<, >|b]`) are highlighted.
- **Engine-true trailing separators:** recognised exactly where the engine extracts them —
  before a `|`, never before the closing `]` — including literal brace separators
  (`[x<{a|b}>|y]`), while HTML-ish forms (`[a</b>|c]`, `[a<br/>|c]`) stay content.
- Directives highlight after closed inline comments on the same logical line
  (`/# note #/#set %x% = 1`), because the engine strips comments before reading directives.
- Strict conditional / plural openers — `{??x}` and `{plural noun}` are *not* mis-highlighted.
- Block-comment toggle (`/# … #/`) and auto-closing of `{}`, `[]`, `%%`, `""`.

## Development

The grammar is verified with headless [`vscode-tmgrammar-test`](https://github.com/PanAeon/vscode-tmgrammar-test):

```sh
npm install
npm test
```

Grammar lives in [`syntaxes/spintax.tmLanguage.json`](./syntaxes/spintax.tmLanguage.json);
scope assertions in [`tests/`](./tests). It mirrors the Sublime Text package
([investblog/sublime-spintax](https://github.com/investblog/sublime-spintax)).

## Related

- 🖥️ Spintax Studio, the desktop editor — [Microsoft Store](https://apps.microsoft.com/detail/9mw3ch7b530p) · [source](https://github.com/investblog/spintax-studio)
- 📖 Syntax reference — <https://spintax.net/docs/syntax>
- 🧪 Live playground — <https://spintax.net/play/>
- 📦 Engine (`@spintax/core`) — <https://www.npmjs.com/package/@spintax/core>

## License

[MIT](./LICENSE) — part of the [301.st](https://301.st) toolset.
