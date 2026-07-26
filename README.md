# DriverClear — Offline DOT Fleet Compliance Appliance

**Your fleet's compliance data isn't up in the cloud. It's right here, in your shop.**

DriverClear is a self-contained compliance appliance for small fleets: a dedicated Linux
machine running your DOT/FMCSA recordkeeping — driver pre-trip and post-trip DVIRs, work
orders welded to the defect chain, fleet profiles, driver credentials, short-haul time
records, fuel, and a tamper-evident audit trail. Drivers use it from their own phones on
your shop network. It works with no internet connection at all.

Built and maintained by [A Clear Umbrella, LLC](https://aclearumbrella.com) — a
veteran-owned business.

---

## What This Is

DriverClear is a **physical appliance**, not a cloud service and not a desktop app you
install alongside other software. It's a complete Linux system — the operating system,
the application, the database, the encrypted backups — all on one box in your building.
Every phone, tablet, and computer on your local network reaches it through a browser.
Nothing leaves your network. Nothing phones home.

**DriverClear is not an ELD.** Electronic Logging Devices are a separately regulated,
FMCSA-registered product category. DriverClear does not plug into your ECM and does not
satisfy any ELD mandate. What it keeps is **§395.1(e) short-haul time records** — the
records the short-haul exception actually requires — derived from authenticated check-in
and check-out timestamps.

## What It Does

| Capability | What it means for your operation |
|---|---|
| **DVIRs — Pre-Trip & Post-Trip** | Walk-around inspection on a phone, item by item, with photos. Each DVIR ends CLEARED, DEFECT, or BLOCKED — no scores, no gray area. |
| **Defect → Work Order → Repair Certification** | The §396.11(c) chain, welded shut. A defect opens a work order automatically; the truck can't dispatch until a mechanic certifies the repair. |
| **Fleet Profiles & Glovebox** | Per-asset history (every DVIR, defect, work order) plus a document pocket for registration, insurance, annual inspection, permits. |
| **Driver Credentials & Expiry Tracking** | CDL, medical card, and whatever else you track — with expiration dates that flag before they surprise you roadside. |
| **Short-Haul Time Records (§395.1(e))** | Authenticated check-in/check-out timestamps become the time-record paperwork the short-haul exception requires. |
| **Fuel Records** | Per-truck fuel purchases, logged or imported. |
| **DOT Audit Binder** | Per-truck compliance PDF — DVIRs, defects, work orders, certifications — formatted to hand across a desk. |
| **Roster CSV Import** | Bring your existing driver list in at once instead of typing one by one. |
| **Dispatcher Role** | Operational view of who and what is cleared to roll, without full administrative access. |
| **Tamper-Evident Audit Chain** | Every significant action is HMAC-SHA256 hashed together with the one before it. Alter an old record and the chain breaks — your records can prove their own integrity. |
| **Encrypted Nightly Backups** | Automatic, encrypted with your passphrase, no button to forget. Export to USB for off-machine custody. |
| **Umbrella Link** | Pair with other A Clear Umbrella appliances (e.g., OSHAClear) using a short code. Signed, LAN-only, revocable, never transmitted to us. |
| **Cryptographically Signed Updates** | Every update is signed; the appliance rejects anything that isn't genuine — over the internet or from a USB. |
| **Hardened at Install** | Full-disk encryption, default-deny firewall exposing only the app port, service minimization, automatic OS security updates. |

## Architecture

- **Stack:** Flask · SQLite · Gunicorn · Linux (Mint today, Debian roadmap)
- **Offline-first:** no cloud dependency, no internet requirement, no telemetry
- **Per-device SSL:** internal CA model with phone-install guide
- **Encrypted at rest:** LUKS full-disk encryption, AES-GCM encrypted backups
- **Signed updates:** Ed25519/minisign — appliance rejects unsigned packages
- **Zero outbound by default:** optional email alerts and remote access (ngrok) are opt-in

## System Requirements

| Component | Requirement |
|---|---|
| **Processor** | Any 64-bit PC or laptop, roughly 2012 or newer |
| **Memory** | 4 GB minimum, 8 GB recommended |
| **Storage** | 32 GB or larger (erased and encrypted during install) |
| **Network** | Local network so drivers' phones can reach the app |
| **Internet** | Optional — only for email alerts, updates you request, or remote access |

## Installation

DriverClear ships as a bootable ISO. Write it to a USB stick with
[balenaEtcher](https://etcher.balena.io/) or [Rufus](https://rufus.ie/), boot a
dedicated spare computer from it, and follow the installer. The machine becomes the
appliance. Twenty minutes, start to finish.

> **The installer erases and encrypts the target machine's entire drive.**
> Use a spare or dedicated computer — never someone's everyday work machine.
> Write down your disk encryption passphrase before you type it.
> If lost, the data is permanently unrecoverable.

A 45-day free trial is available at
[driver-clear.com/trial.html](https://driver-clear.com/trial.html) — the complete
product, nothing locked, no account, no card. Trial data carries over when you buy.

## Verify Downloads

Every release is published with a SHA-256 checksum and a minisign signature.

```bash
# Check integrity
sha256sum -c DriverClear_v2.6.7-Trial.iso.sha256

# Verify publisher signature (install minisign first)
minisign -Vm DriverClear_v2.6.7-Trial.iso \
  -P RWQe9d7iVbo4b2sRul3uKSLeefIizpqjHQJLMnuAxecU9x4EW3Ob89qW
```

The same signing key signs update packages, so the appliance only ever accepts the
genuine article.

## Updates

Updates install two ways: over the internet when you ask (the appliance checks only on
demand and installs nothing without your approval), or from a USB with no internet at
all. Your first year of updates is included with purchase. After that, new versions are
offered as optional paid upgrades at a discounted price for existing customers. What you
bought keeps working either way — perpetual license, no kill switch, no server that has
to stay alive for your records to open.

## The Quadra Family

DriverClear is one of four compliance appliances from A Clear Umbrella:

| Product | Regulator | Site |
|---|---|---|
| **OSHAClear** | OSHA | [osha-clear.com](https://osha-clear.com) |
| **DriverClear** | DOT / FMCSA | [driver-clear.com](https://driver-clear.com) |
| **CaptainClear** | USCG | Coming soon |
| **PilotClear** | FAA | Roadmap |

Each maps to one federal regulator. Each runs independently. Paired over Umbrella Link,
they share rosters and hold each other's encrypted backups — on your network, signed end
to end, never transmitted to us.

## Licensing

DriverClear is proprietary software sold under a perpetual license. This repository
contains the application source as delivered to customers; it is not open source and is
not licensed for redistribution, modification, or derivative works. See
[Product EULA](https://driver-clear.com/product-eula.html) for full terms.

## Documentation

- [Welcome & User Guide](https://driver-clear.com/guide.html) — the complete walkthrough
- [FAQ](https://driver-clear.com/faq.html)
- [Free 45-Day Trial](https://driver-clear.com/trial.html)
- [Product EULA](https://driver-clear.com/product-eula.html)
- [Terms & Privacy](https://driver-clear.com/terms.html)

## Support

DriverClear is built and supported by a small veteran-owned shop. During the pilot
program, when you email you reach the person who wrote the code.

- **Website:** [driver-clear.com](https://driver-clear.com)
- **Parent brand:** [aclearumbrella.com](https://aclearumbrella.com)
- **Contact:** [Request a quote or ask a question](https://driver-clear.com/#quote)

---

*Your fleet's compliance data isn't up in the cloud. It's right here, in your shop,
where it belongs.*

© 2026 A Clear Umbrella, LLC · Veteran-Owned Business

