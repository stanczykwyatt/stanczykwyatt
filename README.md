# Wyatt Stanczyk

I build native macOS apps in Swift, command line tools for coding agents, and web apps in TypeScript, and I like working close to the platform: Carbon hotkeys, the App Sandbox, the Keychain, SQLite's C API and raw WebGL2 with hand-written shaders.

[wyattstanczyk.com](https://wyattstanczyk.com) · [Copy 2FA](https://stanczykwyatt.github.io/copy-2fa/) · [Show Desktop](https://stanczykwyatt.github.io/show-desktop/) · [Who Stole My Focus](https://stanczykwyatt.github.io/who-stole-my-focus/) · [AI Footprint](https://stanczykwyatt.github.io/ai-footprint/) · [mulligan](https://stanczykwyatt.github.io/mulligan/) · [symbolhop](https://stanczykwyatt.github.io/symbolhop/)

## Free Mac utilities

Small, private menu bar apps for macOS. Each one is free and open source with zero dependencies. All but Next Departures have no internet access, and it connects only to the transit feeds you use. What's coming next is on the [public roadmap](https://github.com/users/stanczykwyatt/projects/1).

### [<img src="https://github.com/stanczykwyatt/copy-2fa/raw/main/docs/icon.png" width="28" height="28" align="top" alt=""> Copy 2FA](https://github.com/stanczykwyatt/copy-2fa)

[![macOS CI](https://github.com/stanczykwyatt/copy-2fa/actions/workflows/ci.yml/badge.svg)](https://github.com/stanczykwyatt/copy-2fa/actions/workflows/ci.yml)
[![Windows CI](https://github.com/stanczykwyatt/copy-2fa/actions/workflows/windows.yml/badge.svg)](https://github.com/stanczykwyatt/copy-2fa/actions/workflows/windows.yml)
[![Release](https://img.shields.io/github/v/release/stanczykwyatt/copy-2fa)](https://github.com/stanczykwyatt/copy-2fa/releases)
[![License](https://img.shields.io/github/license/stanczykwyatt/copy-2fa)](https://github.com/stanczykwyatt/copy-2fa/blob/main/LICENSE)

Copies the newest verification code from Mail or Messages, or from a built-in TOTP/HOTP authenticator, with one keyboard shortcut. Swift 6 on macOS 14+, zero dependencies, plus a C#/.NET port for Windows.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="profile/copy-2fa-picker-dark.png">
  <img src="https://github.com/stanczykwyatt/copy-2fa/raw/main/docs/screenshots/picker-light.png" width="380" alt="Copy 2FA's authenticator picker showing live codes">
</picture>

- **No network entitlement.** It runs in the App Sandbox, and CI inspects the built app with `codesign` on every push and fails if a network entitlement ever appears.
- **No Accessibility permission.** The global shortcut uses Carbon's `RegisterEventHotKey`, and Mail is driven over Apple Events after checking consent with `AEDeterminePermissionToAutomateTarget`.
- **Parsers written from scratch:** a `typedstream` decoder for message bodies in `chat.db`, opened read-only through the SQLite C API, and a protobuf wire-format reader for Google Authenticator exports. They live in a pure `Copy2FACore` target that is stress-tested with thousands of damaged inputs.
- **Secrets stay on the Mac.** OTP keys live in a non-synced Keychain item behind Touch ID, codes are computed with CryptoKit HMAC (SHA-1/256/512), and HOTP counters are saved before a code is copied.
- **Clipboard hygiene.** `.currentHostOnly` keeps codes off Universal Clipboard, `ConcealedType` and `TransientType` keep them out of clipboard managers, and `sharingType = .none` hides the notice from screenshots and screen sharing.

### [<img src="https://github.com/stanczykwyatt/show-desktop/raw/main/docs/icon.png" width="28" height="28" align="top" alt=""> Show Desktop](https://github.com/stanczykwyatt/show-desktop)

[![CI](https://github.com/stanczykwyatt/show-desktop/actions/workflows/ci.yml/badge.svg)](https://github.com/stanczykwyatt/show-desktop/actions/workflows/ci.yml)
[![Release](https://img.shields.io/github/v/release/stanczykwyatt/show-desktop)](https://github.com/stanczykwyatt/show-desktop/releases)
[![License](https://img.shields.io/github/license/stanczykwyatt/show-desktop)](https://github.com/stanczykwyatt/show-desktop/blob/main/LICENSE)

The Windows+D shortcut for the Mac: one click or ⌃⇧D hides every window, and the next puts back exactly what was open. Swift 6 and AppKit on macOS 14+, zero dependencies.

<img src="https://github.com/stanczykwyatt/show-desktop/raw/main/docs/demo.gif" width="380" alt="Show Desktop hiding three windows to reveal the Desktop, then bringing them back">

- **Restores the exact state.** It records which apps were visible and orders them back to front from `CGWindowListCopyWindowInfo`, so the second click unhides only those, leaves apps you'd already hidden alone and puts the app you were using back in front. That logic is a pure `ShowDesktopCore` target with its own tests.
- **Works around macOS's activation rules.** Hiding the frontmost app makes macOS promote the next one, so Finder is activated first and stragglers get a second pass.
- **Least privilege.** App Sandbox with no network or file entitlements, a single Apple Events exception for Finder (window IDs only, never user text, go into the scripts), and a Carbon hot key instead of Accessibility. CI fails the build if a network or file entitlement ever appears.

### [<img src="https://github.com/stanczykwyatt/who-stole-my-focus/raw/main/docs/icon.png" width="28" height="28" align="top" alt=""> Who Stole My Focus](https://github.com/stanczykwyatt/who-stole-my-focus)

[![CI](https://github.com/stanczykwyatt/who-stole-my-focus/actions/workflows/ci.yml/badge.svg)](https://github.com/stanczykwyatt/who-stole-my-focus/actions/workflows/ci.yml)
[![Release](https://img.shields.io/github/v/release/stanczykwyatt/who-stole-my-focus)](https://github.com/stanczykwyatt/who-stole-my-focus/releases)
[![License](https://img.shields.io/github/license/stanczykwyatt/who-stole-my-focus)](https://github.com/stanczykwyatt/who-stole-my-focus/blob/main/LICENSE)

Names the app that jumps in front of your work, including in the middle of typing, from a menu bar log of every app that comes to the front. Swift 6, AppKit and SwiftUI on macOS 14+, zero dependencies. Also on **[Windows](https://github.com/stanczykwyatt/who-stole-my-focus-windows)** as a preview, with the same rules in C# and .NET 8.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/stanczykwyatt/who-stole-my-focus/raw/main/docs/screenshot-dark.png">
  <img src="https://github.com/stanczykwyatt/who-stole-my-focus/raw/main/docs/screenshot-light.png" width="560" alt="The Who Stole My Focus log: top offenders, then each steal with the app, what it took focus from, why and for how long">
</picture>

- **No permissions at all.** Detection uses only `NSWorkspace` activation, launch, hide and Space notifications plus `CGEventSource.secondsSinceLastEventType` idle times, which macOS gives sandboxed apps without Accessibility or Input Monitoring. CI fails the build if any entitlement besides the App Sandbox, or any event tap, AX, screen-capture or networking call, appears.
- **Telling steals from switches.** A pure `FocusCore` target picks the likeliest cause from input timing (a click with no typing since, the instant ⌘ comes up after ⌘Tab, a keypress in a launcher), walks a short history of recent fronts so focus returning after a password prompt or the lock screen isn't blamed, and takes a second look 0.4 s later: did you keep typing, did the Space change, did the last app quit after all.
- **Tested off screen.** 170+ Swift Testing tests, including the system monitor fed a private `NotificationCenter` and the SwiftUI log rendered from windows that are never shown. The README screenshots come from those renders.

### [<img src="https://github.com/stanczykwyatt/ai-footprint/raw/main/docs/icon.png" width="28" height="28" align="top" alt=""> AI Footprint](https://github.com/stanczykwyatt/ai-footprint)

[![CI](https://github.com/stanczykwyatt/ai-footprint/actions/workflows/ci.yml/badge.svg)](https://github.com/stanczykwyatt/ai-footprint/actions/workflows/ci.yml)
[![Release](https://img.shields.io/github/v/release/stanczykwyatt/ai-footprint)](https://github.com/stanczykwyatt/ai-footprint/releases)
[![License](https://img.shields.io/github/license/stanczykwyatt/ai-footprint)](https://github.com/stanczykwyatt/ai-footprint/blob/main/LICENSE)

See what AI coding tools leave on your Mac: disk per tool, background processes, orphaned MCP servers and login items, with a before-and-after receipt for every update. Read-only, sandboxed, no internet access. Swift 6, AppKit and SwiftUI on macOS 14+, zero dependencies.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/stanczykwyatt/ai-footprint/raw/main/docs/screenshot-dark.png">
  <img src="https://github.com/stanczykwyatt/ai-footprint/raw/main/docs/screenshot-light.png" width="560" alt="The AI Footprint window: total disk use of AI tools, an orphaned MCP server, then each tool with its folders, processes and login items">
</picture>

- **Read-only by construction.** The only entitlements are the App Sandbox and a read-only list of named subfolders, never a whole tool folder, so tokens, keys and settings stay out of reach. A test fails if a whole folder is ever added, and CI fails the build if any other entitlement, network call or file read outside one reviewed file appears.
- **Processes without extra access.** It lists processes through `sysctl` (`KERN_PROC_ALL`, `KERN_PROCARGS2`), which the sandbox allows, walks parent chains to say what keeps each one alive, and keeps only names from command lines, never arguments or environment variables.
- **Update receipts.** A pure `FootprintCore` target compares each check with the last one and turns version, folder size and login item changes into receipts, measuring folders with `lstat` so symbolic links can't lead it outside what it may read. 186 Swift Testing tests.

### More Mac apps

- <img src="https://github.com/stanczykwyatt/memo-export/raw/main/docs/icon.png" width="20" height="20" align="top" alt=""> **[Memo Export](https://github.com/stanczykwyatt/memo-export)**: your Voice Memos transcripts, as Markdown with the audio. Timestamped paragraphs, only new memos each run, Obsidian-friendly. Read-only and sandboxed. [Website](https://stanczykwyatt.github.io/memo-export/)
- <img src="https://github.com/stanczykwyatt/podcast-export/raw/main/docs/icon.png" width="20" height="20" align="top" alt=""> **[Podcast Export](https://github.com/stanczykwyatt/podcast-export)**: your Apple Podcasts shows as OPML, and your listening history as notes, CSV and JSON. Read-only and sandboxed. [Website](https://stanczykwyatt.github.io/podcast-export/)
- <img src="https://github.com/stanczykwyatt/reading-list-export/raw/main/docs/icon.png" width="20" height="20" align="top" alt=""> **[Reading List Export](https://github.com/stanczykwyatt/reading-list-export)**: your Safari Reading List as Markdown, bookmarks and CSV, read status included, for Wallabag, Linkding, Readwise Reader or your notes. Read-only and sandboxed. [Website](https://stanczykwyatt.github.io/reading-list-export/)
- <img src="https://github.com/stanczykwyatt/unblock-eject/raw/main/docs/icon.png" width="20" height="20" align="top" alt=""> **[Unblock Eject](https://github.com/stanczykwyatt/unblock-eject)**: see what's keeping your disk from ejecting. Every app and process with files open on the disk, a polite Quit for each, then Eject. Found through `libproc`, ejected through Disk Arbitration; never force-quits. [Website](https://stanczykwyatt.github.io/unblock-eject/)
- <img src="https://github.com/stanczykwyatt/next-departures/raw/main/docs/icon.png" width="20" height="20" align="top" alt=""> **[Next Departures](https://github.com/stanczykwyatt/next-departures)**: your stop's next bus and train, live, in the menu bar. Delays, cancellations and alerts from any GTFS-realtime feed, with its own protobuf decoder; ready for Metro Transit and UMN campus buses. [Website](https://stanczykwyatt.github.io/next-departures/)
- <img src="https://github.com/stanczykwyatt/right-folder/raw/main/docs/icon.png" width="20" height="20" align="top" alt=""> **[Right Folder](https://github.com/stanczykwyatt/right-folder)**: Open and Save dialogs, in the right folder. The folders open in Finder, recent folders and favorites beside every dialog, one click or ⌃⌥J away. A free Default Folder X alternative that uses only Accessibility. [Website](https://stanczykwyatt.github.io/right-folder/)
- <img src="https://github.com/stanczykwyatt/tonecap/raw/main/docs/icon.png" width="20" height="20" align="top" alt=""> **[Tonecap](https://github.com/stanczykwyatt/tonecap)**: design your keyboard's sound. Mechanical keyboard sounds made from numbers, no recordings: a [Web Audio playground](https://stanczykwyatt.github.io/tonecap/) with shareable links and WAV export, and a menu bar app that plays your sound as you type.

## Tools for coding agents

Free, open-source command line tools for Codex, Claude Code, Gemini CLI and other terminal agents. Both make no network calls and have zero dependencies.

### [<img src="https://github.com/stanczykwyatt/mulligan/raw/main/docs/icon.png" width="28" height="28" align="top" alt=""> mulligan](https://github.com/stanczykwyatt/mulligan)

[![CI](https://github.com/stanczykwyatt/mulligan/actions/workflows/ci.yml/badge.svg)](https://github.com/stanczykwyatt/mulligan/actions/workflows/ci.yml)
[![Release](https://img.shields.io/github/v/release/stanczykwyatt/mulligan)](https://github.com/stanczykwyatt/mulligan/releases)

Undo for coding agents. A snapshot at every prompt and tool call, shell commands included, so you can list, diff and undo back to any point. One Swift binary for macOS and Linux that needs only git.

- **Never touches your repo.** Snapshots go into a separate git repository outside the project, with every inherited `GIT_*` variable, global config and hook shut out.
- **Restore can't lose a file.** It snapshots first, fails closed, and leaves alone anything it couldn't put back: ignored files, big files, nested repos.

### [<img src="https://github.com/stanczykwyatt/symbolhop/raw/main/docs/icon.png" width="28" height="28" align="top" alt=""> symbolhop](https://github.com/stanczykwyatt/symbolhop)

[![CI](https://github.com/stanczykwyatt/symbolhop/actions/workflows/ci.yml/badge.svg)](https://github.com/stanczykwyatt/symbolhop/actions/workflows/ci.yml)
[![Release](https://img.shields.io/github/v/release/stanczykwyatt/symbolhop)](https://github.com/stanczykwyatt/symbolhop/releases)

Go-to-definition and find-references for coding agents: an MCP server and CLI that asks your language servers instead of grepping. Node 20+, no dependencies.

- **Its own LSP client.** It picks a server per file (clangd, pyright, gopls, rust-analyzer and more), runs several at once, and takes symbols by name instead of column.
- **Measured, not guessed.** On a made-up C project a function's uses came back as 10 exact lines against 59 to 77 from grep, and CI reruns the measurement.

## Science tools

Free tools for neuroscience data, made for labs that share NWB, HDF5, MATLAB and NumPy files.

- <img src="https://github.com/stanczykwyatt/brainhound/raw/main/docs/icon.png" width="20" height="20" align="top" alt=""> **[brainhound](https://github.com/stanczykwyatt/brainhound)**: search and read public neuroscience data from your coding agent. One search across DANDI, OpenNeuro and NeuroMorpho, NWB files read remotely a slice at a time through its own read-only HDF5 reader, and a license and citation with every dataset. MCP server, CLI and skills. [Website](https://stanczykwyatt.github.io/brainhound/)
- <img src="https://github.com/stanczykwyatt/dropslice/raw/main/docs/icon.png" width="20" height="20" align="top" alt=""> **[dropslice](https://stanczykwyatt.github.io/dropslice/)**: open NWB, HDF5, MATLAB, NumPy and Zarr files in your browser, with plots, images and tables. Nothing is uploaded. Built on h5web. [Source](https://github.com/stanczykwyatt/dropslice)
- <img src="https://github.com/stanczykwyatt/scilook/raw/main/docs/icon.png" width="20" height="20" align="top" alt=""> **[SciLook](https://github.com/stanczykwyatt/scilook)**: Quick Look for scientific data on your Mac. Press Space on NWB, HDF5, MATLAB or NumPy files to see their structure and a plot. Sandboxed, no permissions, no internet access. [Website](https://stanczykwyatt.github.io/scilook/)

## Website

### [wyattstanczyk.com](https://wyattstanczyk.com)

My personal site: Next.js 16 and React 19 with no other runtime dependencies, deployed on Vercel.

- **Raw WebGL2, no 3D library.** A ray-marched SDF scene in hand-written GLSL renders into half-float HDR framebuffers, followed by a post pass with bloom, ACES tone mapping and dithering.
- **HDR reflections** from an equirectangular environment map, decoded by a hand-written Radiance `.hdr` (RGBE with run-length encoding) loader and blurred by roughness through its mip chain.
- **Cache Components** (`"use cache"` with `cacheLife("hours")`) for live GitHub data, and a strict Content Security Policy with security headers on every route.
- Respects `prefers-reduced-motion` and caps the device pixel ratio at 2.

## Toolbox

<img src="profile/toolbox.svg" height="40" alt="Swift, TypeScript, C#, Bash, Next.js, React, Tailwind CSS, Vercel, Supabase, PostgreSQL, SQLite, GitHub Actions, Git">

<table>
  <tr><td><b>Languages</b></td><td>Swift · TypeScript · JavaScript · C# · GLSL · SQL · Bash</td></tr>
  <tr><td><b>Apple</b></td><td>SwiftUI · AppKit · Carbon · Security · LocalAuthentication · CryptoKit · App Sandbox · Swift Package Manager</td></tr>
  <tr><td><b>Web</b></td><td>Next.js · React · Tailwind CSS · WebGL2 · Node.js</td></tr>
  <tr><td><b>Agents</b></td><td>MCP · Language Server Protocol · agent hooks · Agent Skills</td></tr>
  <tr><td><b>Science data</b></td><td>NWB · HDF5 · MATLAB MAT-files · NumPy · Zarr · h5web · JavaScriptCore</td></tr>
  <tr><td><b>Data and infra</b></td><td>Supabase (Postgres) · SQLite · Vercel · GitHub Actions · GitHub Pages</td></tr>
</table>

## GitHub stats

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="profile/stats-dark.svg">
  <img src="profile/stats.svg" height="170" alt="GitHub stats: commits, pull requests and issues">
</picture>
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="profile/streak-dark.svg">
  <img src="profile/streak.svg" height="170" alt="GitHub contribution streak">
</picture>

<sub>Cards are rendered daily by a <a href="https://github.com/stanczykwyatt/stanczykwyatt/blob/main/.github/workflows/profile-cards.yml">GitHub Actions workflow</a> in this repo rather than a shared public instance.</sub>
