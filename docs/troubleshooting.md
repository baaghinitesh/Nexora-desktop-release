# 🆘 Troubleshooting & Support Guide

This guide covers solutions to the most common issues encountered when setting up or using the Nexora Remote Control suite.

---

## 🔍 Connection Issues Checklist

If your mobile app cannot connect to the desktop server, go through these steps in order:

### 1. Are they on the same network?
- Check that your phone and PC are connected to the same Wi-Fi SSID.
- If using Mobile Hotspot, ensure your phone is connected to the PC's hotspot network.
- Disable any active VPN on **both** your phone and your PC.

### 2. Verify the IP Address
- Use the IP address shown on the Nexora Desktop dashboard.
- If multiple IP addresses are listed, try each one (they correspond to different adapters like Ethernet, Wi-Fi, and Virtual Hotspot).

### 3. Run the Network Health Check
- Open a web browser on your phone.
- Enter the URL: `http://<YOUR_PC_IP>:3000/health` (replace `<YOUR_PC_IP>` with the IP shown in the app).
- **Result**:
  - If you see `{"status":"ok", ...}`, the HTTP server is running and accessible. The issue is likely with WebSockets (port 8080) or Bluetooth.
  - If the page fails to load, the server is blocked by a firewall or is not running.

---

## 🛡️ Windows Firewall & Network Category Auditing

Windows Firewall blocks inbound connections on **Public** networks. For Nexora to function, the network adapter must be categorized as **Private** and firewall rules must be present.

### Step 1: Auto-Fix
In Nexora Desktop, go to **Settings** → **Firewall Configuration** and click **Auto Fix**. This prompts for Administrator approval (UAC) to automatically configure your rules.

### Step 2: Manual PowerShell Commands
If auto-fix fails, open PowerShell as an Administrator and execute:
```powershell
# Open ports 3000 and 8080
New-NetFirewallRule -DisplayName "Nexora WebSocket Server" -Direction Inbound -Protocol TCP -LocalPort 8080 -Action Allow -Profile Any
New-NetFirewallRule -DisplayName "Nexora HTTP Server" -Direction Inbound -Protocol TCP -LocalPort 3000 -Action Allow -Profile Any

# Check rules were added
Get-NetFirewallRule -DisplayName "Nexora*" | Select-Object DisplayName, Enabled, Profile
```

### Step 3: Fix Hotspot "Public" Category Profile
Windows classifies hotspot connections as "Public" by default. Run this PowerShell command as Administrator to force it to Private:
```powershell
Get-NetConnectionProfile | Where-Object { $_.NetworkCategory -eq 0 } | ForEach-Object { Set-NetConnectionProfile -InterfaceIndex $_.InterfaceIndex -NetworkCategory Private }
```

---

## 🔌 Port Conflict Resolutions

If the Nexora server fails to start, another application may be using its ports:
- **Port 3000**: Used for HTTP API uploads/status
- **Port 8080**: Used for WebSockets remote events

### How to identify and terminate conflicting processes:

1. Open Command Prompt as Administrator.
2. Find the Process ID (PID) using the port:
   ```cmd
   netstat -ano | findstr :3000
   netstat -ano | findstr :8080
   ```
3. Terminate the blocking process (replace `<PID>` with the number found at the far right of the netstat output):
   ```cmd
   taskkill /PID <PID> /F
   ```
4. Restart Nexora Desktop.

---

## 🔄 Auto-Update Troubles

### The app is not updating automatically
- Ensure your PC is connected to the internet.
- Ensure that the GitHub Releases page `baaghinitesh/Nexora-desktop-release` is accessible.
- If you are on a restricted network (corporate proxy or VPN), the auto-update endpoint may be blocked.
- You can manually download the latest installer directly from the [Official Releases Page](https://github.com/baaghinitesh/Nexora-desktop-release/releases).

---

## 📧 Support and Contact

If you have tried all the troubleshooting steps and still need help:
- 🌐 **Visit our Support Page**: [launchyourconcept.com](https://www.launchyourconcept.com/)
- ✉️ **Send us feedback or bug reports**: [launchyourconcept.com/contact](https://www.launchyourconcept.com/contact)
