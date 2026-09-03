# Network Architecture & Auto-Discovery

[← Back to Main Documentation](../README.md)

Nexora utilizes a zero-configuration local networking engine that enables phones and PCs to discover and pair instantly without requiring static IP configuration.

---

## Network Architecture & Port Map

```
┌────────────────────────────────────────────────────────────────────────┐
│                        NEXORA PORT ALLOCATION                          │
├───────────────┬──────────┬─────────────────────────────────────────────┤
│ Port          │ Protocol │ Purpose                                     │
├───────────────┼──────────┼─────────────────────────────────────────────┤
│ TCP 3000      │ HTTP     │ REST API, QR code generation, File Uploads  │
│ TCP 8080      │ WS       │ Real-time events, Keyboard, Clipboard sync  │
│ UDP 8081      │ UDP      │ Ultra-low latency mouse movement streaming  │
│ UDP 5353      │ mDNS     │ Multicast DNS automatic zero-config discover│
└───────────────┴──────────┴─────────────────────────────────────────────┘
```

---

## Discovery & Pairing Methods

```mermaid
flowchart TD
    Start[User Opens Mobile App] --> Choice{Discovery Method}
    Choice -->|Method 1| QR[Scan QR Code on PC Screen]
    Choice -->|Method 2| mDNS[Auto-Discover Host via mDNS :5353]
    Choice -->|Method 3| Manual[Enter Local IP + 4-Digit PIN]
    Choice -->|Method 4| Hotspot[Auto-Detect PC Mobile Hotspot 192.168.137.1]
    
    QR --> Pair[Pairing Verification API :3000]
    mDNS --> Pair
    Manual --> Pair
    Hotspot --> Pair
    
    Pair --> Connect[Establish Persistent WS :8080 & UDP :8081 Channels]
```

### 1. Instant QR Code Pairing (Recommended)
- The desktop app displays a dynamic QR code on the Dashboard.
- Scanning the QR code with the mobile camera transfers the exact IP address, port, and security token in one step.

### 2. Zero-Configuration mDNS Discovery
- The desktop host advertises the `_nexora._tcp.local` service via multicast DNS (Bonjour).
- The mobile app automatically scans the local network and lists active Nexora hosts.

### 3. Manual IP & Security PIN
- Manually enter your computer's local IPv4 address (e.g. `192.168.1.105`) and the 4-digit security PIN displayed on your PC screen.

### 4. PC Mobile Hotspot Mode (Offline / No Wi-Fi Router)
- Turn on **Mobile Hotspot** in Windows Settings (*Network & Internet → Mobile hotspot*).
- Connect your phone to your PC hotspot.
- Nexora automatically detects the default gateway (`192.168.137.1`) for instant connection.

---

[← Back to Main Documentation](../README.md)
