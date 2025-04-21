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

> A test policy is created in the firewall and placed in the top of list to

2. **Set Application Filter**
3. **Configure Web Filter**
4. **Restrict Gmail Logins (non-whitelisted domains)**
5. **Apply to test systems and monitor**

### ❗ Challenges Faced:
- Managing **exceptions** for trusted domains/sites
- Need for **SSL inspection certificate** installation across endpoints

---

## 🧩 Active Directory Configuration (via GPO)

- Restrict user privileges
- Disable Bluetooth file sharing
- Block browser extension installations
- Disable mobile hotspot sharing

---

## ☁️ Google Workspace DLP Settings

- Enable protection against phishing attacks
- Allow email communication only with approved domains
- Restrict Drive file sharing to trusted domains

---

## ✅ Conclusion

This project leverages existing tools to establish an effective, policy-driven DLP framework while maintaining ISO 27001:2022 and ISO 13485 compliance.

---

## 📸 Screenshots

> 📝 _Each step will be documented with images in the respective folders:_

- `screenshots/firewall/`
- `screenshots/active_directory/`
- `screenshots/google_workspace/`

Example:

```markdown
### FortiGate Policy Setup

![Firewall Policy Creation](screenshots/firewall/firewall_policy_creation.png)

### GPO Bluetooth Restriction

![GPO Bluetooth Block](screenshots/active_directory/gpo_bluetooth_block.png)
