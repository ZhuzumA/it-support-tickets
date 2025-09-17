# Ticket: Password Reset/Account Locked
**Date:** 2025-09-17  
**Category:** Network/Windows  

---

## 1) Problem Description
- **User report:** "I can't access the internet. Browser shows: *No Internet* and Teams won't connect."
- **Impact:** User is completly blocked - cannot acces emails, Teams, or internal recources that require internet/VPN.  

---

## 2) Environment
- OS: Windows 10 Enterprise
- Hardware: Dell (domain joined laptop) 
- Network: Corporate LAN (Ethernet)  
- User account type: AD domain account  
- Other users: Reported that others nearby still habe internet access
- Relevant systems: VPN not in use (issue happens before VPN connection)

---

## 3) Reproduction Steps
1. Open any browser - attempt to access a website (e.g. https://www.google.com).
2. Browser shows "No Internet/DNS_PROBE_FINISHED_NO_INTERNET".
3. Teams and Outlook fail to connect.

---

## 4) Diagnostics / Troubleshooting
- **Checked physical connection:** Ethernet cable connected, link light on. 
- **Verified IP configuration:**
```bash
ipconfig /all #no default gateway assigned.
ipconfig /release 
ipconfig /renew #renewed IP adress
ping 8.8.8.8 #OK
ping google.com #Fails (DNS resolution issue)
ipconfig /flushdns #flushed DNS cache
```
- **Retested - and still failling.**
-**Changed DNS temporary to 8.8.8.8/8.8.8.4 in adapter settings**
-**Internet worked after change.**
---

## 5) Root Cause
DHCP provided correct IP but DNS server was not responding. Issue limited to this user's previous DNS settings.

---

## 6) Solution / Fix
- Reconfigured network adapter to use reliable DNS servers (8.8.8.8and 1.1.1.1).  
- Restarted network adapter and confirmed internet connection restored.

---

## 7) Verification
- User successfully loaded multiple websites.
- Outlook and Teams reconnected automatically.
- Verified stable cennectivity.

---

## 8) Post-Mortem / Prevention
- Documented issue for network team to investigate internal DNS server outage. 
- Recommended adding monitoring/alerting on DNS service health. 
- Added steps for local DNS troubleshooting to internal knowledge base. 

