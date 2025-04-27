# 🔐 Data Leak Prevention (DLP) Implementation in IT Infrastructure

## 📌 Table of Contents
- [Overview](#overview)
- [Firewall-Level Controls (FortiGate)](#firewall-level-controls-fortigate)
  - [Implementation Steps](#implementation-steps)
  - [Challenges Faced](#challenges-faced)
- [Active Directory Configuration](#active-directory-configuration)
- [Google Workspace DLP Settings](#google-workspace-dlp-settings)
- [Conclusion](#conclusion)
- [Screenshots](#screenshots)

---

## 📘 Overview

This project outlines the implementation of Data Leak Prevention (DLP) within a corporate IT infrastructure using FortiGate Firewall, Active Directory, and Google Workspace. The primary objective is to safeguard confidential data and prevent unauthorized transfers from employees' systems to external destinations.

1. Personal or external email logins
2. Phishing attacks (spear phishing and generic)
3. Uncontrolled file sharing via Google Drive
4. Access to peer-to-peer or cloud-based storage websites
5. VPN usage via browser extensions
6. Remote desktop or peer-to-peer applications (e.g., torrent apps)
7. Bluetooth file transfers

This project also supports the organization's compliance with ISO/IEC 27001:2022 and ISO 13485 standards by ensuring data confidentiality and strengthening security posture.

...

## 🔥 Firewall-Level Controls (FortiGate)

### ✅ Implementatedd Measures
- Application & Website Restriction using Application Control and Web Filter
- Blocking login to non-whitelisted Gmail accounts
- Enabling SSL Deep Inspection for HTTPS traffic

### ✅ Implementation Steps:
1. **Create a Test Policy**
> A test policy is created in the firewall and placed in the top of list to check the outcome.
> Create new Appfilter
> Create new webfilter
> Editing Webfilter for restricting unknown domain gmail login.
> Testing


2. **Set Application Filter**
> Edit the newly created App Filter
> Goto Security -> newly created App Filter
> In Appfilter we can see all the categories with their defualt inspection mode, Monitoring/Block/Allow 
> Click Add new Override and change to selected, make sure action is 'block'
> Search for required apps based on Fortigature Application signature
> Select the categories of Remote access, Peer to Peer, Email, Storage Backup etc
> Choose appropriate apps as selected for overriding the above mentioed category wise settings.
> Set Block for these Apps.
> Save and apply.

![image](https://github.com/user-attachments/assets/78f7c071-c865-4805-aa2b-925c46aa4b1a)


3. **Configure Web Filter**
> Edit the newly created Webfilter.
> Goto Securiity -> newly created webfilter.
> Under the Fortiguard Category Based Filter -> Select the categories to block.
> Create a custom category so that any custom added WEBsites can be overrided based on client requirements or Company specific.
> Save and apply

![image](https://github.com/user-attachments/assets/eb4f6539-8faa-4c9e-9867-23242e8e9aba)


4. **Restrict Gmail Logins (non-whitelisted domains)**
> Enable SSL Deep inspection
> Enable Gmail login protection setting
> Enter the custom domain in the list

<<Screenshot here>>

5. **Apply to test systems and monitor**
<<Screenshot here>>

### ❗ Challenges Faced:
-- Managing **exceptions** for trusted domains/sites

> Goto Webfilter override
> Add the custom URL to the override and give the newly created custom profile

<<Screenshot here>>

> Check the site is applicable

-- Need for **SSL inspection certificate** installation across endpoints

> Download the Firewall certificate
> Goto Active directory, create a new Setting and attach to a new GPO 'Firewall certificate installation'

<<Screenshot here>>

> For Mac Systems, Download the certificate install it to system -> Go to Key editor, make the certificate as trusted always

<<Screenshot here>>

---

## 🧩 Active Directory Configuration (via GPO)

### ✅ Implementatedd Measures
- Limit user privileges to prevent unauthorized software installation.
- Disable Bluetooth file sharing.
- Restrict browser extension installations.
- Block mobile hotspot sharing features on laptops.

1. Disable Bluetooth file sharing

> Create a new setting/policy named as DLP
> Goto 

<<Screenshot here>>

2. Block browser extension installations

> Select the DLP policy
> Goto

<<Screenshot here>>

3. Disable mobile hotspot sharing
> In the DLP policy
> Goto

<<Screenshot here>>

---

## ☁️ Google Workspace DLP Settings

1. Enable protection against phishing attacks

> Login to Google Workspace Admin
> Goto



2. Allow email communication only with approved domains
3. Restrict Drive file sharing to trusted domains

---

## ✅ Conclusion

This project leverages existing tools to establish an effective, policy-driven DLP framework while maintaining ISO 27001:2022 and ISO 13485 compliance.

---
