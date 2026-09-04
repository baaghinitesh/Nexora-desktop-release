# Nexora Ecosystem Architecture & Technical Specification

[← Back to Main Documentation](../README.md)

This document provides a comprehensive architectural breakdown of the **Nexora Wireless Remote Control Ecosystem**, encompassing the **Windows Desktop Host** (Electron / Node.js native engine) and the **Android Mobile Companion** (React Native / Native C++ / Java modules).

---

## 🏗️ High-Level System Architecture

```mermaid
flowchart TB
    subgraph MobileDevice["📱 Android Mobile Companion (React Native / C++ / Java)"]
        UI["User Interface\n• Smart Trackpad\n• 11-Row Mechanical Keypad\n• App & Web Launcher\n• Pen Tablet Canvas\n• Clipboard Manager\n• File Transfer Manager"]
        
        State["Zustand State Store\n• Connection State\n• Active Modifiers\n• Haptic Triggers"]
        
        Speech["Android SpeechRecognizer\n• Continuous Audio Stream\n• Punctuation Normalizer"]
        
        Sensors["Touch & Stylus Engine\n• Touch Velocity Deltas\n• Pressure & Tilt Tracking"]
        
        UI --> State
        Speech --> UI
        Sensors --> UI
        
        NetClient["Network Dispatch Layer"]
        UI --> NetClient
        NetClient -->|UDP Datagrams :8081| UDPClient["UDP Client (react-native-udp)"]
        NetClient -->|WebSocket Packets :8080| WSClient["WebSocket Client (Auto-Reconnect)"]
        NetClient -->|Chunked HTTP Stream :3000| HTTPClient["HTTP Axios / Fetch Client"]
        NetClient -->|mDNS Scanner :5353| MDNSClient["mDNS Discovery Client"]
    end

    subgraph PrivateLAN["🔒 Private Local Area Network (Wi-Fi 5/6 or Windows Mobile Hotspot)"]
        UDPClient -->|Sub-5ms Mouse Movement| SvrUDP
        WSClient -->|Keystrokes, Macros, Clipboard, Strokes| SvrWS
        HTTPClient -->|Pairing API, File Uploads| SvrHTTP
        MDNSClient -->|Service Query '_nexora._tcp.local'| SvrMDNS
    end

    subgraph DesktopHost["💻 Windows Desktop Host (Electron / Node.js / Win32 C#)"]
        SvrMDNS["mDNS Responder\n(Bonjour :5353)"]
        SvrHTTP["HTTP REST API (:3000)\n• Express.js Server\n• Auth Token & 4-Digit PIN\n• Chunked File Receiver"]
        SvrWS["WebSocket Server (:8080)\n• Real-Time Event Bus\n• Heartbeat & Keepalive"]
        SvrUDP["UDP Socket Listener (:8081)\n• High-Frequency Datagrams\n• Fast Packet Unpacker"]
        
        AppRouter["Message Router & Protocol Dispatcher"]
        SvrWS --> AppRouter
        SvrUDP --> AppRouter
        SvrHTTP --> AppRouter
        
        subgraph ExecutionEngines["Native Windows Execution Engines"]
            CS_SendInput["Persistent PowerShell / C# Bridge\n• user32.dll SendInput\n• SendUnicodeString\n• Keyboard & Mouse VK Codes"]
            
            AppLauncherEngine["3-Tier Cascade Launcher\n• Tier 1: shell.openExternal / openPath\n• Tier 2: rundll32 FileProtocolHandler\n• Tier 3: cmd.exe start / Process Spawn"]
            
            ClipboardEngine["Win32 Native Clipboard Bridge\n• Windows Clipboard Monitor\n• Bi-Directional Auto-Sync"]
            
            GhostOverlay["Transparent Ghost Window\n• Frameless Click-Through Window\n• HTML5 2D Canvas Live Mirroring"]
            
            FileSystemEngine["File Manager Pipeline\n• SHA-256 Checksum Engine\n• C:\\Users\\...\\Downloads Storage"]
        end
        
        AppRouter --> CS_SendInput
        AppRouter --> AppLauncherEngine
        AppRouter --> ClipboardEngine
        AppRouter --> GhostOverlay
        AppRouter --> FileSystemEngine
    end

    subgraph WindowsOS["🪟 Windows 10 / 11 Operating System"]
        CS_SendInput --> ActiveApp["Focused Application Window"]
        AppLauncherEngine --> SysApps["Apps, Browsers & Settings"]
        ClipboardEngine --> WinClip["Windows Clipboard"]
        GhostOverlay --> WinDisplay["DirectX / GDI Display Output"]
        FileSystemEngine --> Disk["NTFS File System"]
    end
```

---

## 🌐 Multi-Protocol Communication Stack

Nexora employs a heterogeneous 4-protocol networking model where each task is mapped to the optimal transport layer:

| Port | Protocol | Layer | Purpose | Rationale |
| :--- | :--- | :--- | :--- | :--- |
| **8081** | **UDP** | Transport | Mouse delta tracking & cursor physics | **<5ms latency**. UDP eliminates TCP head-of-line blocking, connection handshakes, and packet retransmission queues. Out-of-order delta packets are superseded by newer packets. |
| **8080** | **TCP / WebSocket** | Application | Keystrokes, modifiers, macros, live pen tablet vector strokes, clipboard sync | **Guaranteed delivery & ordering**. Keystrokes and macros must never be dropped or arrived out of order. |
| **3000** | **TCP / HTTP REST** | Application | Device pairing, QR code verification, 4-digit PIN auth, file uploads, telemetry | **Stateless request/response**. High-throughput chunked HTTP multipart streaming for file transfers up to 40+ MB/s with SHA-256 validation. |
| **5353** | **UDP / mDNS** | Transport | Multicast DNS Zero-Configuration Discovery (`_nexora._tcp.local`) | **Zero manual configuration**. Allows mobile devices to detect desktop hosts across LAN subnets automatically. |

---

## ⚡ Data Flow & Sequence Workflows

### 1. Ultra-Low Latency Mouse Cursor Streaming (UDP)

```mermaid
sequenceDiagram
    autonumber
    participant Touch as Mobile Touch Surface
    participant Filter as Touch Velocity Filter
    participant UDPClient as Mobile UDP Socket (:8081)
    participant UDPServer as Desktop UDP Listener (:8081)
    participant Win32 as Windows user32.dll SendInput
    participant Cursor as Desktop Cursor

    Touch->>Filter: Touch Event (x1, y1) -> (x2, y2)
    Filter->>Filter: Calculate dx, dy & Apply Pointer Acceleration
    Filter->>UDPClient: Packed Byte Payload: [type=1, dx=4, dy=-2, flags=0]
    UDPClient->>UDPServer: Fire-and-forget UDP Datagram
    UDPServer->>Win32: SendInput(MOUSEINPUT { dx: 4, dy: -2, dwFlags: MOUSEEVENTF_MOVE })
    Win32->>Cursor: Instant smooth cursor translation (<3ms total)
```

---

### 2. Full Unicode Keyboard & Voice Dictation Injection

```mermaid
sequenceDiagram
    autonumber
    participant Voice as Mobile Voice Mic / Keycap
    participant Pipeline as Speech Normalizer / Keypad
    participant WS as WebSocket Channel (:8080)
    participant PSBridge as Desktop C# / PowerShell Bridge
    participant Win32 as Windows SendInput
    participant TargetApp as Active App (VS Code, Word, Browser)

    Voice->>Pipeline: Spoken text: "hello world newline this is nexora"
    Pipeline->>Pipeline: Normalizes punctuation: "hello world\nthis is nexora"
    Pipeline->>WS: JSON: { type: "keyboard", payload: { key: "...", action: "press" } }
    WS->>PSBridge: TEXT | base64_utf8_string
    PSBridge->>Win32: SendUnicodeString(KEYBDINPUT { wScan: utf16_char, dwFlags: KEYEVENTF_UNICODE })
    Win32->>TargetApp: Injects exact Unicode characters & line breaks
```

---

### 3. Ghost Window Live Annotation Pipeline

```mermaid
sequenceDiagram
    autonumber
    participant Pen as Mobile Stylus / Touch Canvas
    participant WS as WebSocket Channel (:8080)
    participant Electron as Desktop Electron Main Process
    participant Ghost as Transparent Ghost Window Overlay
    participant Screen as Windows Display Output

    Pen->>WS: Stroke Path Point: { x: 0.45, y: 0.72, pressure: 0.85, color: "#EF4444", size: 4 }
    WS->>Electron: IPC Event: "pen-stroke-data"
    Electron->>Ghost: webContents.send("draw-stroke", point)
    Ghost->>Ghost: HTML5 Canvas 2D ctx.lineTo() & bezierCurveTo()
    Ghost->>Screen: Direct transparent hardware-accelerated render
```

---

## 🔒 Security & Sandboxing Model

1. **Strict Local Network Isolation**:
   - The desktop host binds to local network interfaces (`0.0.0.0` or private subnets `192.168.x.x`, `10.x.x.x`, `172.16.x.x`).
   - No external WAN port forwarding, STUN/TURN relays, or cloud servers are utilized.
2. **Session Authentication**:
   - Device pairing is authenticated using a dynamic 4-digit security PIN or cryptographic QR verification token.
3. **Memory Safety & Process Isolation**:
   - High-privilege Win32 API calls (`SendInput`, `ShellExecuteEx`) are executed within a dedicated child process bridge with clean boundary checking.
4. **Zero Keystroke Telemetry**:
   - Neither the mobile client nor the desktop server logs, stores, or transmits user input strings outside the active LAN socket session.

---

[← Back to Main Documentation](../README.md)
