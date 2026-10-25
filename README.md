<p align="center"><img src="icon.svg" width="88" alt=""></p>

<h1 align="center">OZ — Oz Protocol</h1>

<p align="center">
  An anonymous peer-to-peer network that runs in a browser tab or a small desktop app.<br>
  Onion routing over WebRTC, anonymous boards, and large-file sharing with no central server.
</p>

<p align="center">
  <a href="https://github.com/jkfujinami/Oz/releases/latest"><b>Download</b></a> ·
  <a href="TERMS.md">Terms of use</a> ·
  <a href="README.ja.md">日本語</a>
</p>

> [!WARNING]
> OZ is experimental and has not been audited. Do not rely on it where your safety is at stake.

By using OZ you agree to the [Terms of Use](TERMS.md) (in Japanese). In short: follow the laws of the country where you use it, do not share anything that infringes copyright or is otherwise illegal, and use it at your own risk. The app asks for your agreement the first time it starts.

This repository hosts the releases. The source code is not public at the moment.

## What it does

- **Onion routing inside the browser.** Each message goes through two hops (a Guard picked from your WebRTC neighbours, then a middle node picked from the Guard's neighbours) under layered encryption. Traffic is cut into fixed 1,200-byte cells and mixed with constant cover traffic, so your Guard cannot tell when you actually send something, and the middle node never learns your IP.
- **Everyone relays.** There are no volunteer exit operators. An open tab carries cells for others the way they carry yours.
- **Posts don't start at you.** After the middle node, a post takes a Dandelion++-style random walk before it is broadcast.
- **Anonymous boards.** Public boards and private boards shared by a code. Each thread uses its own signing key, so your posts in different threads cannot be linked.
- **Large files with no storage server.** Files are encrypted, Reed-Solomon coded (any 16 of 32 shards rebuild a stripe) and spread across free public file hosts. Uploads and downloads are done by middle nodes on your circuits through Cloudflare Workers that participants run themselves, so neither the hosts nor the relays see your IP.
- **Abuse resistance without accounts.** Node identities cost an Argon2id proof of work, and every post carries an Equi-X proof of work scaled to its size.
- **Local data stays sealed.** History and board keys are encrypted with your passphrase. There is a lock and a panic-wipe button.

## What it does not protect against

- A global passive adversary that watches most of the internet.
- Your Guard colluding with one of your middle nodes. This is the same two-hop limit Tor has. Trusted Guards reduce the risk.
- Your Guard and the tracker see your IP, so they know you use OZ. The tracker only introduces WebRTC peers.
- Cloudflare sits under every relay Worker. It sees middle nodes and file-host URLs, not you, but it is one company.
- File hosts can delete files. Half the shards of each stripe can disappear and the file still rebuilds.

## Install

Get the installer for your system from [Releases](https://github.com/jkfujinami/Oz/releases/latest) and check it against `SHA256SUMS.txt`:

```sh
shasum -a 256 -c SHA256SUMS.txt --ignore-missing
```

The builds are **not code-signed**, because signing ties an Apple or Microsoft developer account to a real identity.

- **macOS:** open the `.dmg` (`arm64` for Apple silicon), drag OZ to Applications, then right-click it and choose **Open** the first time.
- **Windows:** run `OZ-<version>-setup.exe`. When SmartScreen warns about an unknown publisher, choose **More info → Run anyway**.
- **Linux:** `chmod +x OZ-*.AppImage` and run it.

The desktop app can also run a **Guard** for other people (Settings → この端末の Guard). It runs in the background under its own identity and keeps running while the window is locked.

## Setting up file sharing

File sharing needs a relay Worker of your own on Cloudflare Workers. It is free and needs no credit card.

Download `OZ-worker-<version>.zip` from [Releases](https://github.com/jkfujinami/Oz/releases/latest) and follow `README.txt` inside (Japanese). You can paste the code into the Cloudflare dashboard without installing anything, or install Node.js and run `deploy-worker.bat` (Windows) or `deploy-worker.sh` (macOS and Linux). Then paste the Worker URL into Settings → ファイル共有 in OZ.

The Worker URL is shared with other participants, so use a Cloudflare account that is not tied to your real name or everyday email.

## Trusted Guards

Your first hop sees your IP, so OZ prefers Guards from a list signed by the maintainer. The maintainer's public key is built into the app:

```
2IQuxnQDHTX90kQX459JutUg9rpdkD0GDUTZ-_1bjNk
```

Until a signed list exists, clients choose Guards from the open network.

## Security

Report vulnerabilities privately as described in [SECURITY.md](SECURITY.md), not in public issues.

## License and terms of use

The software is licensed under the [MIT License](LICENSE), and its use is governed by the [Terms of Use](TERMS.md). OZ is provided as is, without warranty. You are responsible for how you use it and for complying with the laws that apply to you.
