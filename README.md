# shiv

An AI assistant that runs in your terminal, on a jailbroken iPhone.

Talk to a model, let it run commands, keep the conversation across sessions. Sign in to OpenRouter through the browser, or paste your own key for any other provider. Since 2.0 it can also build tweaks, end to end, without leaving the phone.

**This repository is for releases and bug reports. shiv is closed source — no code here.**

## Install

Add the repo in Sileo:

```
https://muratkurt.github.io/
```

Then install **shiv**. Node.js is bundled; nothing else to add.

Packages are also attached to each [release](../../releases).

## Requirements

- iOS 15 or newer
- **rootless** (Dopamine / Procursus) → the `iphoneos-arm64` package
- **RootHide** → the `iphoneos-arm64e` package
- Your own API key, or an OpenRouter account for browser sign-in

Built and used on iPhone 15 Pro (iOS 17.0.3, RootHide) and iPhone XS (iOS 16.1.1, Dopamine rootless). Rootful jailbreaks are untested.

## Providers

**OpenRouter** signs in through the browser — no key to copy — and reaches Claude, GPT, Gemini and Grok with one account.

Also built in: DeepSeek · OpenAI · Moonshot (Kimi) · xAI (Grok) · Qwen · Z.ai (GLM) · Groq · Mistral · Together AI · a local Ollama or llama.cpp · any custom OpenAI-compatible endpoint.

## What it does

- **Reads and edits files, runs shell commands.** Shell commands and file writes always ask first; what counts as "ask every time" is set in `/permissions`.
- **Sessions** are saved every turn. Resume the last one, pick an earlier one, or rewind the conversation to an earlier point.
- **Profiles** keep several providers side by side, switchable without losing the conversation.
- **Tweak and reverse engineering tools** — class and method lookup from the device's own dyld cache, a live class dump from a running app, FairPlay decryption on the phone, and an environment probe that runs each tool instead of looking for it.

## Getting started

```
shiv
```

First launch asks for language, provider, API key and model.

Run it as **mobile**, not as root. Under root the files it writes end up owned by root and `~/.shiv` stops being writable.

**OpenRouter — no key to copy.** Pick OpenRouter in the wizard and the browser sign-in starts by itself: Safari opens the approval page, you confirm with your OpenRouter account, and the key comes back to the app. If the browser cannot be opened automatically, type the short address it prints (`http://127.0.0.1:<port>`) into any browser on the device.

**Any other provider** — paste your key at the key step.

Settings live in `~/.shiv` and survive upgrades. Sessions are stored locally in `~/.shiv/oturumlar`.

## Permissions

Shell commands and file writes always ask first. What counts as "ask every time" is set in `/permissions`, and `/preferences` covers the rest of the behaviour.

Reading files, listing directories and searching don't ask.

## Commands

Command names follow the interface language (`/lang`), so `/login` and `/giriş` are the same command. A prefix is enough — `/prof`.

| Command | What it does |
|---|---|
| `/help` | command list |
| `/login` | sign in with a new profile (setup wizard) |
| `/key` | show the key · refresh · remove and sign out |
| `/profile` | switch, add or delete profiles |
| `/model` | pick a model; the list comes from the provider |
| `/new` · `/clear` | new session · clear the screen |
| `/continue` · `/resume` | resume the last session · pick an earlier one |
| `/export` | export the session to a file |
| `/context` · `/notes` | what is loaded · your context notes file |
| `/dir` · `/add-dir` | working directory |
| `/settings` · `/preferences` · `/permissions` | interface · behaviour · command approval |
| `/performance` | drawing and streaming speed |
| `/theme` · `/lang` · `/name` | interface |
| `/geo` | status: version, model, session, limits |
| `/env` | development environment probe |
| `/device` | device scan: iOS, model, jailbreak, disk |
| `/mac` · `/remote` | set up and check building on a Mac over the network |
| `/about` · `/licence` · `/quit` | |

Press **Esc** to stop a running answer. **Esc again** opens rewind, which takes the conversation back to an earlier point.

## Shortcuts

```
↑ ↓      history            Ctrl+R   search history
Ctrl+T   show output        Ctrl+G   jump to bottom (fullscreen)
Ctrl+B   background a job   Esc      stop
/        commands           ?        shortcut list
```

A job sent to the background keeps running while you type. Press `↓` to reach the counter at the bottom, `Enter` for detail, `k` to stop it, `←` to go back to the list, `Esc` to close.

## Tweak and reverse engineering

shiv measures the device instead of guessing. Start here:

```
/env
```

Reports theos, the SDK, frida, `oldabi`, the inject directory and the bundled tools — each one actually run, not just looked for, so a broken tool shows as broken.

On PATH after install:

```
shiv-sinif <name>          class/method search from the dyld cache (no frida)
shiv-oku <bundle-id>       live ObjC class dump from a running app (frida)
shiv-cek <bundle-id>       FairPlay decrypt, on the device itself (frida)
shiv-frida-kur             diagnose frida: binary, package, agent, daemon,
                           conflicts and leftovers
```

`TWEAK.md` ships with the package and holds the recipes the assistant follows.

### frida

`shiv-oku` and `shiv-cek` need **frida-server 16.x** — the client is linked against frida-core 16, so a 17.x server cannot talk to it.

`shiv-frida-kur` reports what you have and changes nothing. It installs the matching 16.1.4 only when you ask:

```
sudo shiv-frida-kur --kur
```

Rootless takes the GitHub release deb; RootHide takes it from the RootHide repo, which is unsigned and needs `--allow-unauthenticated`. The version is then held so an update can't replace it with 17.x.

### Building

Tweaks for App Store apps and command line tools build on the phone with nothing to think about.

System process tweaks are arm64e, and the on-device compiler emits an older pointer authentication ABI. Two things in your code trip it: an `@"..."` string literal, and handing a **block** to the system. Hooks, selector calls and ivar reads are fine.

`oldabi` bridges both, and `/env` reports whether you have it. It does not cover **Swift** — the Swift runtime authenticates its own pointers somewhere `oldabi` never reaches, so a Swift arm64e tweak built on the phone still crashes. That one has to be built on a Mac.

For a package other people will install, declare it:

```
Depends: firmware (<< 15.0) | cy+cpu.arm64v8 | oldabi
```

Or connect a Mac with `/mac` and build there for a deb that depends on nothing.

## Environment variables

```
SHIV_OLUK='╰' shiv        change the gutter glyph if your terminal
                          draws the default one at the wrong width
SHIV_ANLATIM_KATLA=1      fold the assistant's narration too
SHIV_NODE_CAPRAZ=1        build debs on a Mac with no device attached
```

## Reporting a bug

Open an [issue](../../issues/new/choose). The form asks for your device, iOS version, jailbreak and shiv version — without those a report usually can't be acted on.

## Credits

Built with [Claude Code](https://claude.com/claude-code).

## Licence

Closed source. All rights reserved. The packages are free to install and use; redistribution and reverse engineering are not permitted.
