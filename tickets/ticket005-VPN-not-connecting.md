# Ticket: VPN Not Connecting
**Date:** 2025-09-20  
**Category:** Network / VPN  

---

## 1) Problem Description
- **User report:**  
  "When I try to connect to the VPN, I get the error: *'Cannot reach VPN server'* and I cannot access internal company resources."  
- **Impact:**  
  User cannot access intranet, shared drives, or internal applications while working remotely, blocked from completing daily tasks.

---

## 2) Environment
- OS: Windows 10 
- Device: Dell (company laptop)  
- VPN Client: Cisco 
- Network: Home Wi-Fi (private network, stable internet)  
- Other users: No reported VPN issues (problem seems user-specific)  

---

## 3) Reproduction Steps
1. Launch Cisco AnyConnect VPN client.  
2. Enter VPN gateway address and click "Connect."  
3. Receive error message: *"Connection attempt has failed due to network or PC issue."*

---

## 4) Diagnostics / Troubleshooting
- **Verified internet connection:**  
  ```bash
  ping 8.8.8.8
  ping company.com #both successful, so internet is fine
  ```
- checking DNS:
```bash
nslookup vpn.company.com #returned correct IP adress
```
- Checked local firewall: Windows Defender Firewall running, but no custom block rules.
- Collected VPN logs from AnyConnect for further analysis.
- Verified user credentials:
- Password worked on Office 365 and Teams (not expired).
- Account not locked in AD.
- Reset VPN adapter:
```bash
netsh int ip reset
ipconfig /flushdns
ipconfig /renew #retried VPN connection, success
```

## 5) Root Cause
Corrupted local network adapter state was blocking VPN tunnel establishment. 

## 6) Solution 
- Reset IP stack and flushed DNS cache.
- Restarted computer and retested VPN, connected successfully.
- Verified access to intranet and mapped drives.

## 7) Verification
- User confirmed VPN connection stable for more than 15 minutes.
- Able to open internal sites and mapped network drive.

## 8) Prevention
- Recommended user to keep VPN client updated and avoid forced disconnects.
- Logged incident for trend monitoring — if more users report similar issue, IT will check VPN gateway logs for errors.