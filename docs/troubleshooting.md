# Troubleshooting & FAQ Guide

[← Back to Main Documentation](../README.md)

Common troubleshooting steps and solutions for connection, firewall, and performance issues.

---

## Quick Diagnostic Matrix

| Issue | Root Cause | Solution |
| :--- | :--- | :--- |
| **Phone cannot find PC** | Devices on different subnets or Guest Wi-Fi isolation | Connect both PC and phone to the same 2.4GHz or 5GHz Wi-Fi band. Ensure AP Isolation is disabled on the router. |
| **Connection timed out** | Windows Defender Firewall blocking port 8080/3000 | Open Nexora Desktop > Settings > Network & Firewall and click **Auto Setup**. |
| **Public Network Profile** | Windows treats Wi-Fi as Public and drops incoming packets | Open Nexora Desktop > Settings > click **Fix to Private** network profile. |
| **Cursor stuttering** | Network congestion on 2.4GHz band | Switch router to 5GHz Wi-Fi or enable PC Mobile Hotspot mode. |
| **Clipboard not syncing** | Android background app restrictions | Open the Nexora mobile app or tap **Push to Phone** in the desktop Clipboard tab. |

---

## Step-by-Step Resolution Guides

### 1. 1-Click Auto Setup (Desktop App)
1. Open **Nexora Desktop**.
2. Navigate to **Settings** > **Network & Firewall Optimization**.
3. Click **⚡ 1-Click Auto Setup**.
4. Accept the Windows UAC elevation prompt. All firewall rules and network profile configurations will be applied automatically.

### 2. Manual Windows Firewall Setup (PowerShell Admin)
If you prefer configuring firewall rules manually, run PowerShell as Administrator and execute:

```powershell
netsh advfirewall firewall add rule name="Nexora WebSocket (8080)" dir=in action=allow protocol=TCP localport=8080
netsh advfirewall firewall add rule name="Nexora HTTP API (3000)" dir=in action=allow protocol=TCP localport=3000
netsh advfirewall firewall add rule name="Nexora UDP Mouse (8081)" dir=in action=allow protocol=UDP localport=8081
```

### 3. Change Active Wi-Fi Profile to Private
Run PowerShell as Administrator:

```powershell
Get-NetConnectionProfile | Set-NetConnectionProfile -NetworkCategory Private
```

---

## Technical Support

For further assistance or to submit bug reports:
- **Official Website**: [launchyourconcept.com](https://www.launchyourconcept.com/)
- **Contact Form**: [launchyourconcept.com/contact](https://www.launchyourconcept.com/contact)

---

[← Back to Main Documentation](../README.md)
