# Spintax for VS Code

[![Version](https://img.shields.io/visual-studio-marketplace/v/301st.spintax?label=Marketplace)](https://marketplace.visualstudio.com/items?itemName=301st.spintax)
[![Installs](https://img.shields.io/visual-studio-marketplace/i/301st.spintax)](https://marketplace.visualstudio.com/items?itemName=301st.spintax)
[![CI](https://github.com/investblog/vscode-spintax/actions/workflows/ci.yml/badge.svg)](https://github.com/investblog/vscode-spintax/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)

Syntax highlighting for [**spintax**](https://spintax.net) templates in Visual Studio Code —
engine-accurate against the [`@spintax/core`](https://www.npmjs.com/package/@spintax/core)
contract and verified headlessly with `vscode-tmgrammar-test`. Applies to `.spintax` and
`.gtw` files.

| Construct | Example |
| --- | --- |
| Enumeration | `{a\|b\|c}` |
| Permutation | `[<minsize=2;maxsize=3;sep=", ">a\|b\|c]` |
| Variable | `%name%` |
| Local set | `#set %name% = value` |
| Include | `#include "slug-or-id"` |
| Conditional | `{?VAR?then\|else}` · `{?!VAR?…}` |
| Plural | `{plural %n%: one\|few\|many}` |
| Comment | `/# … #/` |

## Example

```spintax
/# hero block #/
#set %product% = Acme
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
- **Correct permutation config:** `<minsize=…;sep=…>` is config, while HTML inside items
  (`[<li>a</li>|b]`, `[<a href="/x">…</a>|b]`) is content — not mis-highlighted as config.
  Genuine separators (`[<and>a|b]`, `[a<, >|b]`) are highlighted.
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

- 📖 Syntax reference — <https://spintax.net/docs/syntax>
- 🧪 Live playground — <https://spintax.net/play/>
- 📦 Engine (`@spintax/core`) — <https://www.npmjs.com/package/@spintax/core>

## License

[MIT](./LICENSE) — part of the [301.st](https://301.st) toolset.
