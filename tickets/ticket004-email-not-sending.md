# Ticket: Email Not Sending / Receiving
**Date:** 2025-09-18  
**Category:** Outlook / Office 365 / Exchange  

---

## 1) Problem Description
- **User report:**  
  "I’m not receiving any new emails since this morning, and when I try to send one, it stays in Outbox."  
- **Impact:**  
  User cannot communicate with team or customers. High business impact.

---

## 2) Environment
- OS: Windows 10 Enterprise   
- Device: Dell  
- Outlook Version: Microsoft 365 Apps for Enterprise (latest)  
- Mail Server: Exchange Online (Office 365)  
- Other users: No reported issues in the same department  

---

## 3) Reproduction Steps
1. Open Outlook → inbox shows no new mail after 09:30.  
2. Send a test message to colleague → message stuck in Outbox.  
3. Colleague confirms they never received it.

---

## 4) Diagnostics / Troubleshooting
- **Checked Outlook status:**  
  Status bar showed "Disconnected."  
- **Verified network connection:**  
  - `ping outlook.office365.com` → OK  
  - Browser access to [https://outlook.office.com](https://outlook.office.com) → works.  
- **Tried Send/Receive All Folders** → still no sync.  
- **Restarted Outlook** → no effect.  
- **Created new Outlook profile:**  
  - Control Panel → Mail → Show Profiles → Add new → Reconnected mailbox.  
  - Mail started syncing normally.  
- **Checked mailbox quota:**  
  - Within limits (< 50 GB).  

---

## 5) Root Cause
Corrupted Outlook profile cache causing client to lose connection to Exchange server.

---

## 6) Solution / Fix
- Closed Outlook.  
- Renamed OST file to force Outlook to rebuild local cache on next start.  
  (Path: `%localappdata%\Microsoft\Outlook\`)  
- Reopened Outlook → profile rebuilt and mailbox synced.  
- Verified Outbox was empty and new messages could be sent.

---

## 7) Verification
- Sent test email to colleague → received successfully.  
- Received new incoming messages.  
- User confirmed Outlook is working as expected.

---

## 8) Post-Mortem / Prevention
- Educated user on reporting issues early if Outlook shows "Disconnected" status.  
- Added KB article for rebuilding Outlook profile / clearing OST cache.