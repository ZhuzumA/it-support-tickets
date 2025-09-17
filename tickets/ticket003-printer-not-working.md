# Ticket: Printer Not Working/ Not found
**Date:** 2025-09-17  
**Category:** Windows/Network Printer  

---

## 1) Problem Description
- **User report:** ""I cannot print anything - the printer disappeared from my Devices and Printers list. When I try to add it again, I get an error: *Windows cannot connect to the printer.*"
- **Impact:** User cannot print important documents; business impact: delayed workflo for their department.  

---

## 2) Environment
- OS: Windows 10 Enterprise
- Hardware: Dell (domain joined laptop) 
- Network: Corporate LAN (Ethernet)  
- Printer: HP Laser (shared network printer)  
- Other users: Printer works for others in the same department

---

## 3) Reproduction Steps
1. Open "Control Panel - Devices and Printers"
2. Network printer not listed.
3. Attempt to add printer: 
      - Control Panel - Add Printer - Select shared printer
      - Error appears: *"Windows cannot connect to the printer. Operation failed with error"*

---

## 4) Diagnostics / Troubleshooting
- **Checked network connectivity:**
```bash
ping print-srv #successfully response, printer server reachebale
```
- Checked print server share: opened \\print-srv\ in File Explorer - printer share visibale
- **Restarted Print Spooler service:**
```bash
net stop spooler
net start spooler #no errors
del %systemroot%\System32\spool\PRINTERS\* /Q #cleared print queue folder
```
- Checked Event Viewer - System log for print-related errorrs - no critical errors.
- Reinstalled printer drivers: downloaded correct driver from vendor or print server - readded printer:
```bash
rundll32 printui.dll,PrintUIEntry /in /n \\print-srv\HP-Laser
```

---

## 5) Root Cause
Local print spooler was holding corrupted job data, causing printer mapping to fail. After clearing spooler and reinstalling printer driver, mapping succeeded. 

---

## 6) Solution / Fix
- Restarted Print Spooler.
- Cleared queue folder.
- Removed and reinstalled printer with correct driver.
- Tested printing with a sample document.   

---

## 7) Verification
- Test page printed successfully.
- User confirmed they could print from Word and Outlook wihtout issues.

---

## 8) Post-Mortem / Prevention
- Recommended checking Group Policy deployment for printer mapping consistency. 

