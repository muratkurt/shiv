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

Full usage, the command list and the tweak tools are on the [package page](https://muratkurt.github.io/depictions/com.muratkurt.shiv.html).

## Reporting a bug

Open an [issue](../../issues/new/choose). The form asks for your device, iOS version, jailbreak and shiv version — without those a report usually can't be acted on.

## Licence

Closed source. All rights reserved. The packages are free to install and use; redistribution and reverse engineering are not permitted.
