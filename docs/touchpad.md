# Smart Trackpad & Cursor Control Engine

[← Back to Main Documentation](../README.md)

Nexora transforms your smartphone into an ultra-low latency multi-touch trackpad with native text drag-selection, fluid momentum scrolling, and sub-5ms input latency over your private local Wi-Fi network.

---

## Technical Architecture & Sub-5ms UDP Streaming

Cursor movement requires high-frequency polling and instantaneous packet transmission. Nexora transmits movement deltas via **fire-and-forget UDP datagrams** rather than TCP sockets, eliminating head-of-line blocking entirely.

```
+----------------------------------+         +------------------------------------+
|   MOBILE (React Native)          |         |   WINDOWS PC HOST (Electron)       |
|                                  |         |                                    |
|  Touch Surface                   |         |  UDP Listener  :8081               |
|  60–120 Hz touch listener        |         |         |                          |
|         |                        |         |         v                          |
|         | raw delta (dx, dy)     |         |  Fast Packet Unpacker              |
|         v                        |         |         |                          |
|  Pointer Acceleration Filter     |         |         v                          |
|         |                        |         |  Win32  SendInput / mouse_event    |
|         | packed UDP datagram    |         |         |                          |
|         v                        |         |         v                          |
|  UDP Socket Client  :8081  ------+---------+-> Windows Desktop Cursor           |
+----------------------------------+  Wi-Fi  +------------------------------------+
```

**Why UDP for mouse movement?**

```
TCP (not used for mouse)            UDP (used by Nexora)
----------------------------        ----------------------------
Guarantees delivery                 Fire-and-forget — no wait
Retransmits dropped packets         Dropped packet = irrelevant,
  --> adds latency spikes             cursor already moved on
Head-of-line blocking               No blocking — next packet
  --> queuing under load              dispatches immediately
~10–50ms effective latency          < 5ms effective latency
```

Each UDP datagram carries a movement delta `(dx, dy)`. If a packet arrives out of order or is lost, the next packet already has the correct position — no correction needed.

---

## Multi-Touch Gesture Matrix

Nexora delivers full parity with physical laptop trackpads and multi-button mice:

| Gesture | Action | Description |
| :--- | :--- | :--- |
| **1-Finger Swipe** | Cursor Movement | Smooth, high-precision navigation across single or multi-monitor setups |
| **1-Finger Tap** | Left Click | Standard click for focusing elements, opening files, selecting objects |
| **2-Finger Tap** | Right Click | Opens Windows context menus and secondary option popups |
| **3-Finger Tap** | Middle Click | Opens links in new background tabs or activates autoscroll |
| **Double-Tap & Slide** | Drag & Text Selection | Double-tap and hold to drag windows, highlight text, or select blocks |
| **Long-Press (320ms)** | Haptic Drag Lock | Latches the left button down with haptic confirmation for window repositioning |
| **2-Finger Slide** | Scroll | Natural vertical and horizontal scrolling through pages, PDFs, and code |
| **Side Scrollbar** | Fast Document Traversal | Rapidly navigate through 10,000+ line files by sliding the right screen edge |
| **Pinch to Zoom** | Zoom In / Out | Synthesises `Ctrl + Mouse Wheel` to scale web pages and canvas views |

---

## Connection Flow

```
User opens Mobile App
        |
        v
Select discovery method
        |
        +--[Scan QR Code]-----------> Read IP + token from QR  -->  Connect
        |
        +--[Auto mDNS Discovery]-----> Scan LAN for _nexora._tcp.local  -->  Connect
        |
        +--[Manual IP + PIN]---------> Enter 192.168.x.x + 4-digit PIN  -->  Connect
        |
        +--[PC Mobile Hotspot]-------> Auto-detect 192.168.137.1  -->  Connect
                                                |
                                                v
                               Pairing & Auth API  (:3000)
                                                |
                                                v
                               Persistent WS :8080  +  UDP :8081
```

---

## Precision & Sensitivity Settings

Configure trackpad behaviour from the Mobile Settings tab:

| Setting | Range | Effect |
| :--- | :--- | :--- |
| **Tracking Sensitivity** | 0.5x – 3.0x | Linear cursor speed multiplier |
| **Pointer Acceleration** | On / Off | Non-linear velocity curve — fast flick = long travel, slow move = pixel precision |
| **Scroll Inversion** | Natural / Traditional | Natural = macOS-style; Traditional = Windows-style |
| **Haptic Feedback** | Low / Medium / High | Tactile vibration intensity for taps, clicks, and drag lock |

---

[← Back to Main Documentation](../README.md)
