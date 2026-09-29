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
| `com.muratkurt.shivtools` | the tools, the rules file, the recipe book, the guide and four examples |
| `com.muratkurt.nodejs` | Node.js 24 + npm + npx (needed only by `shiv-sinif`) |
| `com.muratkurt.ios-mcp` | an MCP server so an AI agent can see and drive the device |
| `com.muratkurt.frida` | frida 17 for rootless and RootHide (the official build; an existing `re.frida.server` works too) |

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

## Working with an AI agent

shivtools does not include an AI. It gives the one you already run in the terminal (Claude Code, Codex, Gemini…) eyes and hands on the device, and a set of rules learned the hard way.

**Quick start, no project folder.** Just tell the agent:

> shivtools is installed. Find the rules file with `shiv-kurulum --goster`, read it with `cat`, and follow it for this session. Then run `shiv-ortam`.

Say `cat` on purpose: on RootHide the rules live inside the jailbreak root, where some agents' file tools can't see them — the shell can. This lasts for one session; for real work, use a project folder:

**1. Give the project the rules.** Make a folder for your tweak and drop the rules file in it — the agent reads it by itself when it starts there:

```
mkdir -p ~/Documents/MyTweak && cd ~/Documents/MyTweak
shiv-kurulum
```

**2. Optional: let it see the screen.** With the iOS MCP package installed:

```
shiv-mcp baglan
```

prints one `claude mcp add …` line. Run it, then restart the CLI.

**3. Start the CLI in that folder and say what you want.** Plain language is enough. Some first messages that work well:

> Read CLAUDE.md, run `shiv-ortam`, and tell me what this device is ready for.

> I want a SpringBoard tweak that hides the dock icon labels. Start from `shiv-ornek 01`, find the class with `shiv-sinif`, and measure before you change anything.

> My tweak's Settings page is empty. Find out why — measure first, don't change code yet.

> Remove the promoted items from the feed in this app. Look at the screen first and tell me which view or network response carries them.

**While you work**

- **You respring, not the agent.** It will ask. If the agent runs inside a terminal app on the phone, a respring closes it too — reopen the terminal and continue the session (`claude -c`).
- **Tell it what you see.** A flicker or a wrong position rarely shows up in a log; describe it, or take a screen recording and `shiv-kare` turns frames into something it can read.
- **Taps need your OK.** The agent may navigate on its own, but it asks before tapping anything that changes your account, and every time before anything that costs money.
- **After a re-jailbreak** on RootHide, run `shiv-jbroot --yap` **before** starting the CLI, so your sessions and the agent's memory come back.

## Tools

**Environment**

| Tool | What it does |
|---|---|
| `shiv-ortam` | Measures the environment. `--ajan-sina [pid]` checks the whole frida chain end to end |
| `shiv-yol` | The same path in both namespaces (RootHide shell vs. real root) |
| `shiv-kurulum` | Installs the rules file into a project |
| `shiv-jbroot` | After a RootHide re-jailbreak, brings back your AI CLI sessions and memory. Dry run by default |
| `shiv-kur` | What is missing (node, frida, MCP, Theos) and why; `--yap` installs it. The engine behind the setup wizard |
| `shiv-theos-kur` | Checks for the right Theos for your jailbreak and installs it with `--kur` |

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
| `shiv-dugme` | Presses the Action Button: single, double, long. A real press — the assigned action runs |

Every tool prints its own usage with `--help`.

## Settings page and setup wizard

**Settings → shivtools** shows the environment with a live status mark for each part, a one-line summary of the tools (tap for the full list), quick-start commands you can tap to copy, and credits. **↻** at the top right measures again.

**Setup wizard.** When something is missing, an **Install now** row appears at the top. Tap it and you see exactly what will be installed, from which repo and with which command; uncheck what you don't want, tap **Install**, and follow the live log. A small root helper (`shivtools-kurd`) does the install; it accepts only a fixed list of component names and nothing else. Theos asks for your password, so it installs from a terminal:

```
shiv-kur                  # what is missing (changes nothing)
sudo shiv-kur node --yap  # install one component
shiv-theos-kur --kur      # the right Theos for your device
```

The page is available in English, Türkçe, Deutsch, Español, Français, Italiano, Português, Русский and العربية.

## iOS MCP

The optional `com.muratkurt.ios-mcp` package lets an AI agent take screenshots, read the accessibility tree, tap, type, press the Action Button, read the live system log and more.

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
                    04 setup wizard (install engine + root helper + live screen)
```

`shiv-ornek --rehber` prints the paths.

## If shiv is also installed

Older shiv releases linked the same `/usr/local/bin/shiv-*` names to their own copies. Install **shivtools after shiv**; `shiv-ortam` names any tool that is shadowed. shiv 2.5 and later depend on shivtools and no longer do this.

## Reporting a bug

Open an [issue](../../issues/new). Include your device, iOS version, jailbreak, shivtools version and the output of `shiv-ortam` — without those a report usually can't be acted on.

## Credits

Built by muratkurt with [Claude Code](https://claude.com/claude-code) (Claude Opus 5.5).

shivtools stands on these projects:

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

**shivtools** — [MIT](LICENSE). Use it, change it, share it; keep the copyright notice.

**Everything in the table above** keeps its own licence. The licence texts are installed with each package under `/usr/share/doc/`.

**Source for the GPL and AGPL parts** (AppSync Unified, appinst, ldid in the iOS MCP package): they are built unmodified from [witchan/ios-mcp v1.2.8](https://github.com/witchan/ios-mcp/tree/v1.2.8).
