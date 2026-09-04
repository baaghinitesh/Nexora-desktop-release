# Smart Trackpad & Cursor Control Engine

[← Back to Main Documentation](../README.md)

Nexora transforms your smartphone into an ultra-low latency multi-touch trackpad with native text drag-selection, fluid momentum scrolling, and sub-5ms input latency over your private local Wi-Fi network.

---

## ⚡ Technical Architecture & Sub-5ms UDP Streaming

Cursor movement requires high-frequency polling and instantaneous packet transmission. Nexora transmits movement deltas via **fire-and-forget UDP datagrams** rather than TCP sockets:

```mermaid
flowchart LR
    A["📱 Mobile Touch Surface\n(60-120Hz Touch Listener)"] -->|Raw Delta Stream\n(dx, dy)| B["Pointer Acceleration Filter"]
    B -->|Packed UDP Datagram| C["UDP Socket Client :8081"]
    C -->|Local Wi-Fi Network| D["Desktop UDP Listener :8081"]
    D -->|Fast Packet Unpacker| E["Win32 SendInput / mouse_event"]
    E --> F["🖥️ Windows Desktop Cursor Translation"]
```

### Why UDP for Mouse Movements?
- **Zero Head-of-Line Blocking**: Unlike TCP, UDP does not pause the stream to retransmit dropped packets.
- **Sub-5ms Latency**: Packets are dispatched immediately upon finger translation.
- **Superseding Delta Packets**: If an older delta packet arrives out of order, the newer packet has already positioned the cursor correctly.

---

## 🖱️ Multi-Touch Gesture Matrix

Nexora delivers full parity with physical laptop trackpads and multi-button mice:

| Gesture | Action | Description |
| :--- | :--- | :--- |
| **1-Finger Swipe** | **Cursor Movement** | Smooth, high-precision cursor navigation across single or multi-monitor setups |
| **1-Finger Tap** | **Primary Left Click** | Standard button click for focusing elements, opening files, or selecting objects |
| **2-Finger Tap** | **Secondary Right Click** | Opens Windows context menus and secondary option popups |
| **3-Finger Tap** | **Middle Mouse Click** | Opens links in new browser background tabs or activates autoscroll |
| **Double-Tap & Slide** | **Drag & Text Selection** | Double-tap and keep finger held down to drag windows, highlight text in IDEs, or select blocks |
| **Long-Press (320ms)** | **Haptic Drag Lock** | Latches left mouse button down with haptic pulse confirmation for effortless window repositioning |
| **2-Finger Slide** | **Fluid Document Scroll** | Natural vertical and horizontal scrolling through web pages, PDF documents, and code files |
| **Side Scrollbar** | **Infinite Fast Traversal** | Rapidly navigate through 10,000+ line documents by sliding along the right edge of the screen |
| **Pinch-to-Zoom** | **Zoom In / Out** | Synthesizes `Ctrl + Mouse Wheel` events to scale web pages and canvas views |

---

## ⚙️ Precision & Sensitivity Settings

Configure trackpad behavior from the Mobile Settings tab:
- **Tracking Sensitivity**: Adjust linear cursor speed multiplier (0.5x to 3.0x).
- **Pointer Acceleration**: Non-linear velocity curve that accelerates cursor speed during rapid finger flicks while maintaining pixel precision during slow movements.
- **Scroll Inversion**: Toggle between Natural (macOS-style) and Traditional (Windows-style) scroll directions.
- **Haptic Feedback**: Fine-tune tactile vibration intensity for clicks, taps, and drag locks.

---

[← Back to Main Documentation](../README.md)
