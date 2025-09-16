# Ticket: Password Reset/Account Locked
**Date:** 2025-09-16  
**Category:** Windows/Active Directory  

---

## 1) Problem Description
- **User report:** ""I tried to log in several times this morning and now I get a message saying: *Your account has been locked out. Please contact your administrator.*"
- **Impact:** User cannot log in - blocked from all work applications (emails, Teams, shared drives). Business impact: productivity loss for one person.  

---

## 2) Environment
- OS: Windows 10 Enterprise
- Hardware: Dell (domain joined laptop) 
- Network: Corporate LAN (Ethernet)  
- User account type: AD domain account  
- Relevant systems: Active Directory, Office 365, Exchange

---

## 3) Reproduction Steps
1. Power on device and wait for login screen.
2. Enter username/password.
3. System shows error: *"The preferenced account is currently locked out and may not be logged on to"*

---

## 4) Diagnostics / Troubleshooting
- Checked user status in AD Users and Computers - account shows "Locked". 
- Confirmed last login time and number of failed login attempts (Event viewer - Security log)  
- Verified no sign of compromised account (no suspicious logins from other locations).  
- Checked if Caps Lock / wrong keyboard layout could cause repeted failures.

---

## 5) Root Cause
User mistyped password more than 5 times - account locked due to Group Policy (Account Lockout Threshhold - 5 attempts).

---

## 6) Solution / Fix
- Used ADUC (Active Directory User and Computers) - Right click user - **Reset Password**
- Checked **Unlock Account** checkbox.  
- Communicated new password to user over secure channel.   
- Asked user to log in and set a new password at first login.  

---

## 7) Verification
- User was able to log in successfully on domain-joined laptop. 
- Confirmed password change worked and account remained unlocked after rebbot. 

---

## 8) Post-Mortem / Prevention
- Educated user about CapsLock and keyboard layout.
- Set reminder about self-service password reset portal.
- Consider enabling account lockout notifications to IT team to catch repeated brute-force attempts quickly.

