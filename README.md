# Wyatt Stanczyk

I build native macOS apps in Swift and web apps in TypeScript, and I like working close to the platform: Carbon hotkeys, the App Sandbox, the Keychain, SQLite's C API and raw WebGL2 with hand-written shaders.

[wyattstanczyk.com](https://wyattstanczyk.com) · [Copy 2FA](https://stanczykwyatt.github.io/copy-2fa/)

## Projects

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

### [wyattstanczyk.com](https://wyattstanczyk.com)

My personal site: Next.js 16 and React 19 with no other runtime dependencies, deployed on Vercel.

- **Raw WebGL2, no 3D library.** A ray-marched SDF scene in hand-written GLSL renders into half-float HDR framebuffers, followed by a post pass with bloom, ACES tone mapping and dithering.
- **HDR reflections** from an equirectangular environment map, decoded by a hand-written Radiance `.hdr` (RGBE with run-length encoding) loader and blurred by roughness through its mip chain.
- **Cache Components** (`"use cache"` with `cacheLife("hours")`) for live GitHub data, and a strict Content Security Policy with security headers on every route.
- Respects `prefers-reduced-motion` and caps the device pixel ratio at 2.

## Toolbox

<img src="profile/toolbox.svg" height="40" alt="Swift, TypeScript, C#, Bash, Next.js, React, Tailwind CSS, Vercel, Supabase, PostgreSQL, SQLite, GitHub Actions, Git">

<table>
  <tr><td><b>Languages</b></td><td>Swift · TypeScript · C# · GLSL · SQL · Bash</td></tr>
  <tr><td><b>Apple</b></td><td>SwiftUI · AppKit · Carbon · Security · LocalAuthentication · CryptoKit · App Sandbox · Swift Package Manager</td></tr>
  <tr><td><b>Web</b></td><td>Next.js · React · Tailwind CSS · WebGL2</td></tr>
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
