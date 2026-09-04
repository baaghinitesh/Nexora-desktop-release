# Troubleshooting, Diagnostics & FAQ Guide

[← Back to Main Documentation](../README.md)

This guide provides troubleshooting solutions for network connection, Windows Defender Firewall, latency, and permission issues.

---

## 🔍 Quick Diagnostic Matrix

| Symptom | Root Cause | Immediate Solution |
| :--- | :--- | :--- |
| **Phone cannot discover PC** | Devices on different subnets or router AP isolation | Connect both PC and phone to the same 2.4GHz or 5GHz Wi-Fi band. Ensure "AP Isolation" or "Guest Mode" is disabled in your router settings. |
| **Connection timed out** | Windows Defender Firewall blocking port 8080/3000 | Open **Nexora Desktop > Settings > Network & Firewall** and click **⚡ 1-Click Auto Setup**. |
| **Public Network Profile** | Windows treats Wi-Fi as "Public" and drops inbound packets | In Nexora Desktop > Settings, click **Fix to Private** network profile. |
| **Mouse cursor stuttering** | Network congestion on 2.4GHz band | Switch router to 5GHz Wi-Fi or enable Windows PC Mobile Hotspot mode. |
| **Microphone permission alert** | Android audio recording permission not granted | Grant Microphone permission in Android Settings to enable speech-to-text dictation. |
| **Clipboard not syncing** | Android background optimization restrictions | Open the Nexora mobile app or tap **Push to Phone** in the desktop Clipboard dashboard. |

---

## 🛠️ Step-by-Step Resolution Procedures

### 1. 1-Click Auto Setup (Desktop App)
1. Open **Nexora Desktop**.
2. Navigate to **Settings** > **Network & Firewall Optimization**.
3. Click **⚡ 1-Click Auto Setup**.
4. Accept the Windows UAC elevation prompt. All inbound firewall rules and network profile configurations will be applied automatically.

### 2. Manual Windows Firewall Configuration (PowerShell Admin)
If you prefer manual configuration, run PowerShell as Administrator and execute:

```powershell
netsh advfirewall firewall add rule name="Nexora WebSocket (8080)" dir=in action=allow protocol=TCP localport=8080
netsh advfirewall firewall add rule name="Nexora HTTP API (3000)" dir=in action=allow protocol=TCP localport=3000
netsh advfirewall firewall add rule name="Nexora UDP Mouse (8081)" dir=in action=allow protocol=UDP localport=8081
```

### 3. Switch Wi-Fi Network Profile to Private
If Windows classifies your home Wi-Fi as "Public", it blocks inbound discovery packets. Run PowerShell as Administrator:

```powershell
Get-NetConnectionProfile | Set-NetConnectionProfile -NetworkCategory Private
```

### 4. Offline PC Mobile Hotspot Setup (Zero-Router Mode)
If no Wi-Fi router is available:
1. Open Windows **Settings** > **Network & Internet** > **Mobile hotspot**.
2. Turn on Mobile Hotspot.
3. Connect your Android phone to the hotspot network.
4. Open Nexora Mobile — it will automatically detect the desktop host at `192.168.137.1`.

---

## 🏢 Technical Support & Community

For further assistance or bug reports:
- **Official Website**: [launchyourconcept.com](https://www.launchyourconcept.com/)
- **Contact & Support Form**: [launchyourconcept.com/contact](https://www.launchyourconcept.com/contact)

---

[← Back to Main Documentation](../README.md)
