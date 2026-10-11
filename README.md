# LFG-iOS

The iOS companion app for **p99lfg** — the Project 1999 LFG (looking-for-group)
tool. The game itself never runs on iOS; this app is a pure companion: you
indicate you're LFG with a list of characters, classes, and levels you
provide, and see everyone else who's LFG — whether they were marked from the
Windows tool or from a phone.

Everything syncs over **Nostr relays** — no central server, and the same
network the Windows app uses, so listings are shared between all users on
all platforms. The portable C core is the same code that powers the Windows
app ([LeonardChristopher/p99lfg](https://github.com/LeonardChristopher/p99lfg)).

## Features

- **LFG list** — everyone currently LFG on Green or Blue, synced over Nostr
- **Mark LFG manually** — pick your character, class (P99 class list), and
  level 1–65; stays listed until removed or stale (30 min)
- **Cross-device identity** — scan the QR shown by the Windows app (or paste
  the key as a fallback) so your phone and PC publish under the same Nostr
  identity
- **Chat** — encrypted direct messages (NIP-04) with anyone on the LFG list
- **Favourites** — star players to pin them to their own section
- **Ratings** — competent / friendly / listens, 1–5 each, with community
  consensus: a rating is hidden once two active raters disagree with it, so
  malicious raters can't hurt your scores

## Project-chat feature description (copy/paste)

> **LFG-iOS — mobile companion for the P99 LFG tool**
>
> Mark your characters LFG from your phone (name, class, level — no game
> connection needed) and see everyone else who's LFG, synced over the same
> Nostr relays as the Windows tool. Includes encrypted DMs with other
> players, favourites, and community ratings (competent / friendly /
> listens) with consensus-based hiding of malicious ratings. Scan a QR to
> share your identity between PC and phone.

## Build

```sh
brew install xcodegen
cd ios
xcodegen generate
open p99lfg.xcodeproj
```

iOS 15+, Swift 5. Enable automatic signing with your own team in Xcode.

Portable core tests run on any POSIX system (Linux/macOS):

```sh
./build.sh
```

## Layout

- `ios/` — SwiftUI app + XcodeGen project spec + Apple TLS hooks
- `src/` — portable C core shared with the Windows app: log-independent
  entry list, Nostr signing/relay sync, NIP-04 chat, favourites, ratings
- `vendor/` — libsecp256k1 (schnorr + ECDH) and qrcodegen (MIT)
- `tests/` — unit + loopback WebSocket tests (74 checks)
