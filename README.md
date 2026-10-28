# Hi, I'm Wyatt

I build things on the web and for the Mac. Site: [wyattstanczyk.com](https://wyattstanczyk.com)

## Projects

### [<img src="https://github.com/stanczykwyatt/copy-2fa/raw/main/docs/icon.png" width="28" height="28" align="top" alt=""> Copy 2FA](https://github.com/stanczykwyatt/copy-2fa)

[![macOS CI](https://github.com/stanczykwyatt/copy-2fa/actions/workflows/ci.yml/badge.svg)](https://github.com/stanczykwyatt/copy-2fa/actions/workflows/ci.yml)
[![Windows CI](https://github.com/stanczykwyatt/copy-2fa/actions/workflows/windows.yml/badge.svg)](https://github.com/stanczykwyatt/copy-2fa/actions/workflows/windows.yml)
[![Release](https://img.shields.io/github/v/release/stanczykwyatt/copy-2fa)](https://github.com/stanczykwyatt/copy-2fa/releases)
[![License](https://img.shields.io/github/license/stanczykwyatt/copy-2fa)](https://github.com/stanczykwyatt/copy-2fa/blob/main/LICENSE)

Copies the newest verification code from Mail or Messages, or from a built-in TOTP/HOTP authenticator, with one keyboard shortcut. Swift 6 on macOS 14+, zero dependencies, plus a C#/.NET port for Windows.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/stanczykwyatt/copy-2fa/raw/main/docs/screenshots/picker-dark.png">
  <img src="https://github.com/stanczykwyatt/copy-2fa/raw/main/docs/screenshots/picker-light.png" width="380" alt="Copy 2FA's authenticator picker showing live codes">
</picture>

- **No network entitlement.** It runs in the App Sandbox, and CI inspects the built app with `codesign` on every push and fails if a network entitlement ever appears.
- **No Accessibility permission.** The global shortcut uses Carbon's `RegisterEventHotKey`, and Mail is driven over Apple Events after checking consent with `AEDeterminePermissionToAutomateTarget`.
- **Parsers written from scratch:** a `typedstream` decoder for message bodies in `chat.db`, opened read-only through the SQLite C API, and a protobuf wire-format reader for Google Authenticator exports. They live in a pure `Copy2FACore` target that is stress-tested with thousands of damaged inputs.
- **Secrets stay on the Mac.** OTP keys live in a non-synced Keychain item behind Touch ID, codes are computed with CryptoKit HMAC (SHA-1/256/512), and HOTP counters are saved before a code is copied.
- **Clipboard hygiene.** `.currentHostOnly` keeps codes off Universal Clipboard, `ConcealedType` and `TransientType` keep them out of clipboard managers, and `sharingType = .none` hides the notice from screenshots and screen sharing.

### [wyattstanczyk.com](https://wyattstanczyk.com)

My personal site, built with Next.js, React and TypeScript and deployed on Vercel.

## Tech

![Swift](https://img.shields.io/badge/Swift-F05138?style=for-the-badge&logo=swift&logoColor=white)
![SwiftUI](https://img.shields.io/badge/SwiftUI-0A84FF?style=for-the-badge&logo=swift&logoColor=white)
![macOS](https://img.shields.io/badge/macOS-000000?style=for-the-badge&logo=apple&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=nextdotjs&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-000000?style=for-the-badge&logo=vercel&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-1C1C1C?style=for-the-badge&logo=supabase&logoColor=3ECF8E)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)

## GitHub stats

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="profile/stats-dark.svg">
  <img src="profile/stats.svg" height="170" alt="GitHub stats: commits, pull requests and issues">
</picture>
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="profile/streak-dark.svg">
  <img src="profile/streak.svg" height="170" alt="GitHub contribution streak">
</picture>
