<div align="center">

# **N E X O R A**
### Universal PC Remote Control Ecosystem
*Transform your mobile device into a high-performance wireless trackpad, keyboard, launcher & drawing tablet for Windows*

<br />

[![Platform: Windows](https://img.shields.io/badge/Platform-Windows%2010%20%7C%2011%20(64--bit)-0078D4?style=flat-square&logo=windows&logoColor=white)](https://github.com/baaghinitesh/Nexora-desktop-release/releases/latest)
[![Platform: Android](https://img.shields.io/badge/Platform-Android%208.0%2B-3DDC84?style=flat-square&logo=android&logoColor=white)](https://github.com/baaghinitesh/Nexora-desktop-release/releases/latest)
[![Desktop Version](https://img.shields.io/badge/Desktop-v1.0.4-0284c7?style=flat-square)](https://github.com/baaghinitesh/Nexora-desktop-release/releases/latest)
[![Mobile Version](https://img.shields.io/badge/Mobile-v1.0.7-10b981?style=flat-square)](https://github.com/baaghinitesh/Nexora-desktop-release/releases/latest)
[![License: MIT](https://img.shields.io/badge/License-MIT-gray?style=flat-square)](LICENSE)

<br />

<img src="assets/nexora-promo-banner.png" width="920" alt="Nexora Promo Banner" />

<br /><br />

**[Download Windows Desktop App (.exe)](https://github.com/baaghinitesh/Nexora-desktop-release/releases/latest)**
&nbsp;&nbsp;•&nbsp;&nbsp;
**[Download Android Mobile App (.apk)](https://github.com/baaghinitesh/Nexora-desktop-release/releases/latest)**
&nbsp;&nbsp;•&nbsp;&nbsp;
**[Official Website](https://www.launchyourconcept.com)**
&nbsp;&nbsp;•&nbsp;&nbsp;
**[Support & Contact](https://www.launchyourconcept.com/contact)**

</div>

---

## Overview

**Nexora** is an ultra-low latency wireless control ecosystem that turns any Android smartphone into a remote command hub for Windows PCs. Engineered for seamless local communication over Wi-Fi, Nexora gives you complete command over your computer without cables, third-party cloud intermediaries, or hardware dongles.

Nexora is designed, engineered, and maintained by **[Launch Your Concept](https://www.launchyourconcept.com/)**.

<br />

<div align="center">

| Windows Desktop Client | Android Mobile Companion |
| :--- | :--- |
| **Latest Release:** `v1.0.4` | **Latest Release:** `v1.0.7` |
| **Format:** Installer (`Nexora-Setup.exe`) | **Format:** Package (`Nexora-Mobile.apk`) |
| **Requirements:** Windows 10 / 11 (64-bit) | **Requirements:** Android 8.0 (Oreo) or higher |
| **Download:** [Nexora-Setup.exe](https://github.com/baaghinitesh/Nexora-desktop-release/releases/latest/download/Nexora-Setup.exe) | **Download:** [Nexora-Mobile.apk](https://github.com/baaghinitesh/Nexora-desktop-release/releases/latest/download/Nexora-Mobile.apk) |

</div>

---

## Core Capabilities

```
┌─────────────────────────────────────────────────────────────────────────┐
│                           NEXORA SUITE                                  │
├────────────────────┬────────────────────┬───────────────────────────────┤
│   App Launcher     │   Smart Trackpad   │   Advanced Keyboard           │
│   Clipboard Sync   │   Pen Tablet Draw  │   High-Speed File Sharing     │
│   Auto-Discovery   │   Local Security   │   Zero-Cloud Privacy          │
└────────────────────┴────────────────────┴───────────────────────────────┘
```

### 1. One-Tap App & Website Launcher
* **50+ Preset Shortcuts**: Launch browsers, IDEs, games, productivity tools, and Windows utilities with a single tap on your phone.
* **Instant Multi-Tier Execution**: Electron native shell API execution backed by Windows protocol handlers for immediate response.
* **Custom Shortcuts**: Add custom executables, arguments, and URLs with customizable vector icons and color tokens.

### 2. Low-Latency Smart Trackpad
* **Precision Drag-Select**: Native double-tap-and-hold gesture allows you to highlight text, drag windows, and select blocks of code with ease.
* **Sub-5ms Latency**: Real-time UDP streaming ensures buttery-smooth cursor responsiveness without input lag.
* **Full Multi-Touch Gestures**: Two-finger fluid scrolling, pinch-to-zoom, two-finger right-click, three-finger middle-click, and infinite scrollbar.

### 3. Advanced Keyboard & Media Hub
* **Unicode & Emoji Key Injection**: Real-time key injection supports typing all characters, special symbols, and Unicode emojis into any Windows application.
* **Complete Media Suite**: Play/Pause, Next/Previous Track, Stop, Mute, and Volume sliders.
* **Windows Navigation**: Function keys (F1–F12), directional arrow navigation, task switcher (`Alt+Tab`), Task Manager (`Ctrl+Shift+Esc`), Snipping Tool (`Win+Shift+S`), and Lock PC (`Win+L`).

### 4. Bi-Directional Clipboard Sync
* **Universal Real-Time Mirroring**: Copy text on your PC and paste directly on your phone, or copy on your phone and paste on your PC.
* **Local LAN Encryption**: All synchronized text is transmitted locally over your encrypted Wi-Fi session.
* **Clipboard History**: Access previous clips, search history, and push selected items instantly.

### 5. Interactive Pen Tablet & Screen Annotation
* **Live Transparent Overlay**: Mirror drawing strokes from your mobile canvas directly onto an interactive, always-on-top transparent Windows Ghost Window.
* **Stylus Support**: Pressure sensitivity simulation, multi-color palette, customizable stroke widths, grid guides, and one-tap PNG / PDF export.

### 6. High-Speed Local File Sharing
* **40+ MB/s Wi-Fi Transfer**: Chunked streaming pipeline with SHA-256 integrity verification.
* **Direct Explorer Integration**: Transferred photos, videos, and archives land directly in your Windows `Downloads / Nexora Transferred Files` directory.

---

## Technical Architecture

Nexora utilizes an optimized, multi-protocol communication stack designed specifically for private local area networks:

| Protocol / Layer | Port | Function |
| :--- | :--- | :--- |
| **UDP Datagrams** | `8081` | Ultra-low latency mouse tracking (<5ms) with zero packet accumulation |
| **WebSocket (WS)** | `8080` | Real-time bi-directional events (keyboard, clipboard sync, live drawing strokes) |
| **HTTP REST API** | `3000` | Device pairing, QR code generation, network diagnostics, and file transfer streaming |
| **mDNS / Bonjour** | `5353` | Automatic zero-configuration LAN discovery of desktop host |

---

## Quick Start Guide

### Step 1: Install Desktop Host (Windows)
1. Download **`Nexora-Setup.exe`** from the [Latest Release](https://github.com/baaghinitesh/Nexora-desktop-release/releases/latest).
2. Run the installer. Inbound Windows Defender Firewall rules and background services configure automatically.
3. Launch Nexora Desktop to display your local IP and QR pairing code.

### Step 2: Install Mobile Companion (Android)
1. Download **`Nexora-Mobile.apk`** from the [Latest Release](https://github.com/baaghinitesh/Nexora-desktop-release/releases/latest).
2. Install the APK on your Android device.
3. Launch Nexora Mobile, tap **Connect**, and scan the QR code on your PC screen.

---

## Application Showcase

### Desktop Control Hub

| Dashboard View | File Transfers Queue | Settings & Preferences |
| :---: | :---: | :---: |
| ![Desktop Dashboard](screenshots/nexora-desktop1.jpeg) | ![File Transfers](screenshots/nexora-desktop2.jpeg) | ![Desktop Settings](screenshots/nexora-desktop3.jpeg) |

### Mobile Remote Interfaces

| App Launcher & Services | Touchpad with Drag-Select | Advanced PC Keyboard | Pen Tablet Draw |
| :---: | :---: | :---: | :---: |
| ![Mobile Home](screenshots/nexora-mobile1.jpeg) | ![Touchpad](screenshots/nexora-mobile2.jpeg) | ![Keyboard](screenshots/nexora-mobile3.jpeg) | ![Pen Tablet](screenshots/nexora-mobile4.jpeg) |

---

## Privacy & Security

* **100% Local LAN Communication**: All cursor movements, keystrokes, clipboard data, and file transfers stay strictly on your local Wi-Fi router.
* **No Telemetry or Tracking**: No user keystroke logging, cloud uploads, or account registrations required.
* **PIN Authentication**: Secure 4-digit PIN pairing ensures only authorized devices can connect to your host computer.

---

## Developer & Maintenance Credits

Nexora is developed, maintained, and supported by **[Launch Your Concept](https://www.launchyourconcept.com/)**.

<div align="center">

<br />

<a href="https://www.launchyourconcept.com">
  <img src="assets/launch-your-concept-logo.png" width="220" alt="Launch Your Concept" />
</a>

<br /><br />

**[Official Website](https://www.launchyourconcept.com/)** &nbsp;•&nbsp; **[Contact & Support](https://www.launchyourconcept.com/contact)**

<sub>© 2024–2026 Launch Your Concept. All rights reserved.</sub>

</div>
