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

This project describes the implementation of a Data Leak Prevention (DLP) framework using **FortiGate Firewall**, **Active Directory**, and **Google Workspace**.

> ⚠️ Objective: Prevent unauthorized sharing of sensitive data from within the network.

...

## 🔥 Firewall-Level Controls (FortiGate)

### ✅ Implementation Steps:
1. **Create a Test Policy**

> A test policy is created in the firewall and placed in the top of list to check the outcome.
> Create new Appfilter
> Create new webfilter
> Select Proxy mode

<<Screenshot here>>

2. **Set Application Filter**

> Edit the newly created App Filter
> Goto Security -> newly created App Filter
> Select the categories of Remote access, Peer to Peer, Mail webclient, Online storage etc
> Set Block for these categories
> Save and apply

<<Screenshot here>>

3. **Configure Web Filter**

> Edit the newly created Webfilter
> Goto Securiity -> newly created webfilter
> Select the categories to block
> Create a custom category so that any custom added WEBsites can be overrided
> Save and apply

<<Screenshot here>>

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

- Enable protection against phishing attacks
- Allow email communication only with approved domains
- Restrict Drive file sharing to trusted domains

---

## ✅ Conclusion

This project leverages existing tools to establish an effective, policy-driven DLP framework while maintaining ISO 27001:2022 and ISO 13485 compliance.

---
