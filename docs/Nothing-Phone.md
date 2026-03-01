---
title: Nothing Phone Mods
description: Guides for flashing custom ROMs, kernels, and fixing bank apps on Nothing Phone 2.
---

# Nothing Phone Mods

Guides and resources for modding Nothing Phone 2 — custom ROMs, kernels, firmware flashing, and fixing banking/wallet apps.

---

## 📦 Custom ROM & Kernel

### NothingMuchRom + arter97 Kernel

A popular combination for Nothing Phone 2 enthusiasts: a cleaner AOSP-based ROM paired with a performance-tuned kernel.

| Component | Source |
|-----------|--------|
| **NothingMuchRom** | [XDA Forums](https://xdaforums.com/t/nothingmuchrom-for-nothing-phone-2.4623411) |
| **arter97 Kernel** | [XDA Forums](https://xdaforums.com/t/r35-arter97-kernel-for-nothing-phone-2.4631313) |
| **Flashing Guide** | [Telegra.ph Guide](https://telegra.ph/UNOFFICIAL-Flashing--Upgrading-Guide-for-arter97-Nothing-Much-NM-ROM--Kernel-04-21) |

> The Telegra.ph guide covers the full flashing and upgrading workflow for both the ROM and kernel together.

---

## 🏦 Fixing Bank Apps & Google Wallet

Rooted devices or devices with unlocked bootloaders often fail banking app checks. The resources below cover the main approaches.

### Widevine L1 Fix

Restoring Widevine L1 certification (for DRM-protected streaming and some banking verifications) after unlocking the bootloader.

- [Fix Widevine L1 — XDA Forums](https://xdaforums.com/t/fix-widevine-l1-unlocked-bootloader.4731374)

### General Banking App Fixes

| Resource | Description |
|----------|-------------|
| [Reddit Guide (Magisk)](https://www.reddit.com/r/Magisk/comments/1apepfh/article_guide_solving_bank_apps_google_wallet_and) | Comprehensive guide for bank apps, Google Wallet, and Play Integrity on rooted devices |
| [bankingAOSP](https://github.com/lepras/bankingAOSP) | Scripts and Magisk modules to fix banking apps on AOSP-based ROMs |
| [XDA Banking Guide](https://xdaforums.com/t/guide-howto-use-banking-apps-on-your-rooted-device.4530801) | How to use banking apps on rooted devices |

!!! note
    Most banking fixes rely on passing Play Integrity (formerly SafetyNet). The Magisk Hide / Shamiko / Zygisk approach is the most common path.

---

## 🔥 Flashing & Firmware

### Stock Firmware & OTA Updates

Useful if you need to revert to stock, flash incremental OTAs, or recover from a bootloop.

| Resource | Description |
|----------|-------------|
| [Nothing Archive (XDA)](https://xdaforums.com/t/nothing-archive-stock-ota-incremental-updates-firmware-flashing-tools-guides-for-phone-2.4650877) | Stock OTA, incremental updates, firmware, flashing tools, and guides for Phone 2 |
| [spike0en/nothing_archive](https://github.com/spike0en/nothing_archive) | Community-maintained firmware archive |
| [arter97/Nothing_Archive](https://github.com/arter97/Nothing_Archive) | arter97's firmware archive mirror |

### Flashing Tools

| Tool | Description |
|------|-------------|
| [spike0en/nothing_flasher](https://github.com/spike0en/nothing_flasher) | Flasher script for Nothing Phone firmware |
| [spike0en/awesome_nothing](https://github.com/spike0en/awesome_nothing) | Curated list of Nothing Phone resources and tools |

### Bootloop Recovery

If a Magisk module causes a bootloop, you can recover without a full reflash:

- [Fix bootloop caused by modules](https://telegra.ph/Fix-bootloop-caused-by-modules-04-21)

!!! warning
    Always back up your data before flashing. Unlocking the bootloader **wipes the device**.
