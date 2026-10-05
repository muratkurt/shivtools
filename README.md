# shivtools

Tweak development tools that run on the jailbroken iPhone itself. Plain shell tools: any CLI can drive them (Claude Code, Codex, Gemini, or you over SSH).

## Install

Add `https://muratkurt.github.io/` in Sileo and install **shivtools**. Sileo also offers the companion packages; each can be removed on its own.

| Package | Brings |
|---|---|
| `com.muratkurt.shivtools` | the tools, the rules file, the recipe book, the guide and seven examples |
| `com.muratkurt.nodejs` | Node.js 24 (needed only by `shiv-class`) |
| `com.muratkurt.ios-mcp` | an MCP server, so an agent can see and drive the device |
| `com.muratkurt.frida` | frida 17 for rootless and RootHide |

Missing parts can also be installed from **Settings › shivtools**.

## Requirements

- iOS 15 or newer, rootless (`iphoneos-arm64`) or RootHide (`iphoneos-arm64e`)
- A12 or newer for the frida-based tools

## Getting started

```
shiv-env                     # what this device is ready for
cd ~/Documents/MyTweak
shiv-init                    # rules file for your agent (CLAUDE.md, AGENTS.md)
shiv-example                 # working example tweaks to start from
```

Every command has an English and a Turkish name (`shiv-env` = `shiv-ortam`). Every tool prints its usage with `--help`.

With an agent: start it in the project folder and ask in plain language. **Settings › shivtools › Getting started** has ready prompts. On RootHide, run `shiv-jbroot --kur` once so your agent's sessions survive a re-jailbreak.

## Tools

| Tool | What it does |
|---|---|
| `shiv-env` | Measures the environment |
| `shiv-init` | Installs the rules file into a project |
| `shiv-install` | Installs missing or outdated parts |
| `shiv-theos-install` | Installs the right Theos for the device |
| `shiv-class` | Classes, methods and real signatures; `--where`, `--live` |
| `shiv-trace` | Prints a line the moment a method is called |
| `shiv-dump` | Every Objective-C class of a running app |
| `shiv-decrypt` | Decrypts an App Store binary |
| `shiv-screen` | The view tree of the app in front |
| `shiv-window` | SpringBoard's windows and views |
| `shiv-shot` | Screenshots and photos as measurements |
| `shiv-log` | Measurement log for your tweak |
| `shiv-cut` | Deletes a line range safely |
| `shiv-example` | Copies an example tweak |
| `shiv-prefs-example` | How Apple's Settings pages do it |
| `shiv-verify` | Checks a package before you install it |
| `shiv-path` | The same path in both namespaces |
| `shiv-jbroot` | Brings sessions back after a RootHide re-jailbreak |
| `shiv-mcp` | Manages the MCP server |
| `shiv-frida-install` | Checks frida-server |
| `shiv-button` | Presses the Action Button |
| `shiv-back` | Brings the terminal back to the front |

## Guide and examples

Installed under `/usr/local/share/shivtools/`: the rules file, the recipe book, the development guide and seven examples (SpringBoard hook, Settings page, in-app filter, setup wizard, tracker blocker, KASwitcher, measuring tweak). `shiv-example --guide` prints the paths.

## Notes

- The MCP server listens on 127.0.0.1 only and `mcp-root` comes without setuid; both are settings.
- If shiv is also installed, install shivtools after it.

## Bugs

Open an [issue](../../issues/new) with your device, iOS version, jailbreak, shivtools version and the output of `shiv-env`.

## Credits

Built by muratkurt with [Claude Code](https://claude.com/claude-code).

| Project | By | Licence | Ships in |
|---|---|---|---|
| [Node.js](https://nodejs.org) | OpenJS Foundation | MIT | `com.muratkurt.nodejs` |
| [nodejs-for-ios](https://github.com/realAndi/nodejs-for-ios) | andi | MIT | `com.muratkurt.nodejs` |
| [Frida](https://frida.re) | Ole André Vadla Ravnås | wxWindows 3.1 | `com.muratkurt.frida`, `shivfrida` |
| [iOS MCP](https://github.com/witchan/ios-mcp) | witchan | MIT | `com.muratkurt.ios-mcp` |
| [PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR) | PaddlePaddle | Apache-2.0 | `com.muratkurt.ios-mcp` |
| [AppSync Unified](https://github.com/akemin-dayo/AppSync) · appinst | akemin-dayo | GPL-3.0 | `com.muratkurt.ios-mcp` |
| [ldid](https://git.saurik.com/ldid.git) | Jay Freeman (saurik) · Procursus | AGPL-3.0 | `com.muratkurt.ios-mcp` |
| [libplist](https://github.com/libimobiledevice/libplist) | libimobiledevice | LGPL-2.1+ | `com.muratkurt.ios-mcp` |
| [OpenSSL](https://www.openssl.org) | OpenSSL Project | Apache-2.0 | `com.muratkurt.ios-mcp` |

## Licence

shivtools is [MIT](LICENSE). Each project above keeps its own licence; the texts are installed under `/usr/share/doc/`.
