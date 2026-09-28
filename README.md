# shivtools

Tweak development tools that run on the jailbroken iPhone itself.

Measure the device instead of guessing: find classes and read method signatures, dump what is on screen, log a tweak's behaviour, and build, check and install — without a Mac. Plain shell tools, so any CLI can drive them: Claude Code, Codex, Gemini, or you over SSH.

## Install

Add the repo in Sileo:

```
https://muratkurt.github.io/
```

Then install **shivtools**. Sileo also offers the companion packages below; each can be removed on its own without losing shivtools.

| Package | What it brings |
|---|---|
| `com.muratkurt.shivtools` | the tools, the rules file, the recipe book, the guide and three example tweaks |
| `com.muratkurt.nodejs` | Node.js 24 + npm + npx (needed only by `shiv-sinif`) |
| `com.muratkurt.ios-mcp` | an MCP server so an AI agent can see and drive the device |
| `com.muratkurt.frida` | frida 17 for RootHide (rootless users can take frida from `build.frida.re`) |

## Requirements

- iOS 15 or newer
- **rootless** (Dopamine / Procursus) → the `iphoneos-arm64` package
- **RootHide** → the `iphoneos-arm64e` package
- A12 or newer for the frida-backed tools (`shiv-sinif` works everywhere)

Built and used on iPhone 15 Pro (iOS 17.0.3, RootHide) and iPhone XS (iOS 16.1.1, Dopamine rootless). Rootful jailbreaks are untested.

## Getting started

```
shiv-ortam
```

The first thing to run, every session. It prints the jailbreak type, iOS version, theos, SDK, frida, oldabi, node and the MCP server — and it **runs** each tool to check it, instead of only looking for the file.

```
cd /var/mobile/Documents/MyTweak
shiv-kurulum
```

Writes the rules file into the project as `CLAUDE.md` and `AGENTS.md`, so the agent you run there follows them. An existing copy is backed up first, never overwritten.

```
shiv-ornek                 # list the examples
shiv-ornek 02 MyPrefs      # copy one into a new folder
```

Start from a working skeleton instead of an empty file. It refuses to write over an existing folder.

## Tools

**Environment**

| Tool | What it does |
|---|---|
| `shiv-ortam` | Measures the environment. `--ajan-sina [pid]` checks the whole frida chain end to end |
| `shiv-yol` | The same path in both namespaces (RootHide shell vs. real root) |
| `shiv-kurulum` | Installs the rules file into a project |

**Finding the code to hook**

| Tool | What it does |
|---|---|
| `shiv-sinif` | Class and method lookup from the device's own dyld cache. `--tip` reads real method signatures from the running process; `--sdk` tells whether a method is in the public SDK |
| `shiv-oku` | Every Objective-C class of a running app |
| `shiv-cek` | Decrypts an App Store binary on the device (FairPlay) |
| `shiv-prefs-ornek` | Shows how Apple's own Settings pages use a cell or a key |

**Reading the screen**

| Tool | What it does |
|---|---|
| `shiv-ekran` | The view tree of the app in front, as text |
| `shiv-pencere` | SpringBoard's windows and view tree, read-only. `--izle` records changes |
| `shiv-kare` | Turns your screenshots into measurements; only frames taken after the mark are listed |

**Measuring and editing**

| Tool | What it does |
|---|---|
| `shiv-log` | Measurement log: a C snippet for your tweak, then read and summarise |
| `shiv-sil` | Deletes a line range safely: dated backup, diff, comment and brace checks |
| `shiv-ornek` | Copies a working example tweak into your project |

**Device control**

| Tool | What it does |
|---|---|
| `shiv-mcp` | Manages the MCP server: status, on/off, listen mode, connect an agent |
| `shiv-frida-kur` | Checks frida-server; installs only when asked explicitly |

Every tool prints its own usage with `--help`.

## Settings page

**Settings → shivtools** shows the last measurement: environment, every tool with a live status mark, quick-start commands you can tap to copy, and credits. The page reads what `shiv-ortam` measured — run `shiv-ortam` to refresh it.

## iOS MCP

The optional `com.muratkurt.ios-mcp` package lets an AI agent take screenshots, read the accessibility tree, tap, type, read the live system log and more.

```
shiv-mcp durum             # installed? running? which mode?
shiv-mcp baglan            # the `claude mcp add` line for an agent on the phone
shiv-mcp baglan --mac      # SSH tunnel + `claude mcp add` for an agent on a Mac
```

Two defaults are ours, and both are settings, not walls: the server listens on **127.0.0.1 only**, and `mcp-root` is installed **without setuid**. The upstream server has no authentication, so `shiv-mcp mod ag` (listen on the network) asks before it opens anything. From a Mac, use the SSH tunnel instead.

## frida

The tools are built for **frida 17**. On RootHide, frida 17 can bring SpringBoard down on about the third attach in a row; apps are fine, and frida 16 and rootless are not affected. `shiv-ortam` warns about it on RootHide. Batch your SpringBoard work: read once with `shiv-pencere` or `shiv-sinif --tip`, then reuse the dump.

## Guide and examples

Installed with the package:

```
/usr/local/share/shivtools/
  SHIVTOOLS.md      the rules an agent follows
  TWEAK.md          recipes with working code
  rehber/           the development guide (Turkish), and a Mac-only version
  ornekler/         01 SpringBoard hook · 02 Settings page · 03 in-app network filter
```

`shiv-ornek --rehber` prints the paths.

## If shiv is also installed

Older shiv releases linked the same `/usr/local/bin/shiv-*` names to their own copies. Install **shivtools after shiv**; `shiv-ortam` names any tool that is shadowed. shiv 2.5 and later depend on shivtools and no longer do this.

## Reporting a bug

Open an [issue](../../issues/new). Include your device, iOS version, jailbreak, shivtools version and the output of `shiv-ortam` — without those a report usually can't be acted on.

## Credits

Built with [Claude Code](https://claude.com/claude-code) (Claude Opus 5.5).

shivtools stands on these projects, each under its own licence:
[Node.js](https://nodejs.org) (MIT) ·
[nodejs-for-ios](https://github.com/realAndi/nodejs-for-ios) by andi (MIT) ·
[frida](https://frida.re) (wxWindows 3.1) ·
[iOS MCP](https://github.com/witchan/ios-mcp) by witchan (MIT) ·
[PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR) (Apache-2.0) ·
[AppSync Unified](https://github.com/akemin-dayo/AppSync) (GPL-3.0) ·
[ldid](https://git.saurik.com/ldid.git) (AGPL-3.0) ·
[libplist](https://github.com/libimobiledevice/libplist) (LGPL-2.1+) ·
[OpenSSL](https://www.openssl.org) (Apache-2.0)

## Licence

shivtools is released under the [MIT License](LICENSE): use it, change it, share it — keep the copyright notice.

Third-party components keep their own licences, listed above; their licence texts are installed under `/usr/share/doc/`. The iOS MCP package includes GPL-3.0 (AppSync Unified, appinst) and AGPL-3.0 (ldid) parts, built unmodified from [witchan/ios-mcp v1.2.8](https://github.com/witchan/ios-mcp/tree/v1.2.8) — that is where their source is.
