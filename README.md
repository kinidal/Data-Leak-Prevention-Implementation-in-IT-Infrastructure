# 🔐 Data Leak Prevention (DLP) Implementation in IT Infrastructure


## 📘 Goal of this Project

This project outlines the implementation of Data Leak Prevention (DLP) within a corporate IT infrastructure using FortiGate Firewall, Active Directory, and Google Workspace. The primary objective is to safeguard confidential data and prevent unauthorized transfers from employees' systems to external destinations.

1. Personal or external email logins
2. Phishing attacks (spear phishing and generic)
3. Uncontrolled file sharing via Google Drive
4. Access to peer-to-peer or cloud-based storage websites
5. VPN usage via browser extensions
6. Remote desktop or peer-to-peer applications (e.g., torrent apps)
7. Bluetooth file transfers

This project also supports the organization's compliance with ISO/IEC 27001:2022 and ISO 13485 standards by ensuring data confidentiality and strengthening security posture.


## 📌 Table of Contents

- [Firewall-Level Controls (FortiGate)](#-firewall-level-controls-fortigate)
  - [Implementation Steps](#implemented-measures)
  - [Challenges Faced](#challenges-faced)
- [Active Directory Configuration (via GPO)](#-active-directory-configuration-via-gpo)
- [Google Workspace DLP Settings](#-google-workspace-dlp-settings)


---


## 🔥 Firewall-Level Controls (FortiGate)

### ✅ Implemented Measures
- Application & Website Restriction using Application Control and Web Filter
- Blocking login to non-whitelisted Gmail accounts
- Enabling SSL Deep Inspection for HTTPS traffic

### ✅ Implementation Steps
1. **Create a Test Policy**
- A test policy is created in the firewall and placed in the top of list to check the outcome.
- Create new Appfilter
- Create new webfilter
- Editing Webfilter for restricting unknown domain gmail login.
- Testing


2. **Set Application Filter**
- Edit the newly created App Filter
- Goto Security -> newly created App Filter
- In Appfilter we can see all the categories with their defualt inspection mode, Monitoring/Block/Allow 
- Click Add new Override and change to selected, make sure action is 'block'
- Search for required apps based on Fortigature Application signature
- Select the categories of Remote access, Peer to Peer, Email, Storage Backup etc
- Choose appropriate apps as selected for overriding the above mentioed category wise settings.
- Set Block for these Apps.
- Save and apply.

![image](https://github.com/user-attachments/assets/78f7c071-c865-4805-aa2b-925c46aa4b1a)


3. **Configure Web Filter**
- Edit the newly created Webfilter.
- Goto Securiity -> newly created webfilter.
- Under the Fortiguard Category Based Filter -> Select the categories to block.
- Create a custom category so that any custom added WEBsites can be overrided based on client requirements or Company specific.
- Save and apply

![image](https://github.com/user-attachments/assets/eb4f6539-8faa-4c9e-9867-23242e8e9aba)


4. **Restrict Gmail Logins (non-whitelisted domains)**
- Goto newly created Webfilter.
- Make sure it is 'Proxy-based' mode.
- Scroll down to the option Proxy options -> Restrict  Google account usage to specific domains -> Toggle it ON
- Enter the list of company domains and any client specific domain which we want to allow login.
- Please note gmail.com is not a valid entry
- Please note this  setting requires 'SSL Deep inspection' profile  otherwise may not work as expected.
- Go to the newly created firewall policy (where this new webfilter is attached).
- Enable SSL Deep inspection profile
- We will receive a warning regarding SSL inspection is turned ON and that can put up some unexpected challenges.

![image](https://github.com/user-attachments/assets/3ab5caed-0798-4757-a1d7-99d0149e93ae)

![image](https://github.com/user-attachments/assets/c2c7a883-42e6-40c0-ba6c-c70ea94d938c)


5. **Apply to test systems and monitor**


### ❗ Challenges Faced

-- Managing **exceptions** for trusted domains/sites

**Sometime** there are some scenarios are expected like even valid domains are found as restricted domains via firewall due to the fact that they may be newly created domains or these domains are categorized are potentially dangerous categories as per fortigate signatures (or admins set up like that). For overriding this we can create webfilter overrides and 
- Goto Webfilter override
- Create new override, paste the domain or URL and select the appropriate  type wildcard/URL etc
- Select the overriding Categorie as custom category (which we need to create in Locat Categories, and mark that as allowed in the specific webfilter) or any allowed categories already in the webfilter category list of the FOrtiguard category based filter.

<<Screenshot -1 of creating a Webfilter override need to be here>>

![image](https://github.com/user-attachments/assets/bed787c7-551f-476d-b427-2aef7368cf91)



- Check the site if the change is performing as expected.

-- Need for **SSL inspection certificate** installation across endpoints

When the SSL Deep inspection is enabled in a Firewall policy it is more likely that the sites or any URL based connections will undergo strict checking for SSL certificates availability and validity. We may also encounter error from even valid sites because the first certificate validation (symmetric encryption) is happen in from the firewall and the end user system. 

For this to successfully validate we need to install the certificate of the firewall into every systems that connected and accessing via it. For this here i am using Active directory. 

- Download the Firewall certificate.
- Goto Active directory, create a new GPO 'Firewall certificate installation' and attach to the desired Organization Unit.
- Edit GPO ->  Goto

<<Screenshot of certificate adding to trusted root is here>>

- For Mac Systems, Download the certificate install (make sure it is into system)
- Go to Keychain editor, make the certificate as trusted always.

<<Screenshot certificate installation of trusted root systems is here>>

---

## 🧩 Active Directory Configuration (via GPO)

### ✅ Implementatedd Measures

- Disable Bluetooth file sharing.
- Restrict browser extension installations.
- Block mobile hotspot sharing features on laptops.

1. Disable **Bluetooth file sharing**

The ***Aim** is to prevent users from sending and receiving files over bluetooth connection from the Organization owned systems. On the other side users should be able to use Bluetooth connected Headphones and other devices.

- Create a new GPO with a given name and attach to  a desired OU.
- Edit the GPO -> Goto Computer Configuration -> Preferences -> Windows settings -> Registry -> DisableFsquirt -> Set the value 0x1 -> Save
- Edit the GPO -> Computer Configuration -> Policies -> Administrative templates -> System/Device Installation/ Device Installation Restrictions -> Prevent Installation of devices that match any of these Device ID's -> BTH\MF_RFCOMM
- Save and apply
- Apply force sync of Group policy to all systems, also try restart of the endpoint of systems
- Or run the following command in a command prompt window: fsquirt.exe -UnRegister

![image](https://github.com/user-attachments/assets/d359a993-f00b-4944-a8d6-1e0de3267ecc)
![image](https://github.com/user-attachments/assets/9ee4b06b-d49e-486b-b9ef-4b3571f84db2)


2. Block browser extension installations

The Browser extensions can be a significant vulnerability since some extensions can access the systems data and even bypass the security restrictions provided in the network level. We are here concentrating on restriction web browser extensions to Google chrome, Microsoft Edge and firefox since these are only allowed web browsers in the organizations, via GPO.

- For this to achieve we need to download and upload the policy templates respective to that browser.
Refernce URL :  https://support.google.com/chrome/a/answer/187202?hl=en#zippy=%2Cwindows
                https://learn.microsoft.com/en-us/deployedge/configure-microsoft-edge
                https://support.mozilla.org/en-US/kb/customizing-firefox-using-group-policy-windows

- Create new GPO or Edit an existing GPO as preferred. Add to an OU as preferred
- Edit GPO -> Computer Configuration -> Policies -> Administrative templates -> Microsoft Edge Extension
- For disabling all extension enable the item Control which extensions cannot be installed with value *
- For creating exceptions Enable Allow specific extensions to be installed and add the ID of the extension in the list.

![image](https://github.com/user-attachments/assets/0ce92e93-f670-4476-98fd-7655c51d973f)


3. Disable **Mobile hotspot sharing**

Laptops has a feature of sharing its Netowrk connected to other Mobile devices acting as a wireless router on its own. To disable this and saves the unwanted network connections to the outisde network and internal network.

- Create new GPO or Edit an existing GPO as preferred. Add to an OU as preferred
- Edit GPO -> Computer Configuration -> Policies -> Administrative templates -> Network or Network connetions.
- Enable this policy of 'Prohibit use of Internet Connection sharing on your DNS domain network'.

![image](https://github.com/user-attachments/assets/9b493523-9f1f-4e74-b5f5-a36fc0a410fd)


---

## ☁️ Google Workspace DLP Settings

Since Gmail (Google Workspace) serves as the organization's primary communication platform, our primary concern was to safeguard the integrity of email communications and ensure the confidentiality of data stored in Google Drive. The organization has previously encountered spear-phishing attacks, particularly email spoofing attempts where attackers impersonated the CEO and COO. Given these incidents, it is critical to implement enhanced security measures to protect against such threats.


### ✅ Implementatedd Measures

- Multifactor authentication Enroll.
- Phishing email detection and Email spoofing prevention.
- Google Drive File sharing restriction.
- Email Senting Allowlisted domains only.

1. Enable Multifactor Authentication
- Goto Google Worskpace Admin portal -> Security -> Authentication -> 2-Step Verification -> Set the authentication ON. 
- Select the organization in which we want this to enroll..
- We can set the minimum days a user can use the email without 2FA (new users).
- ON/OFF as desired for Allow  for trusted devices option.

![image](https://github.com/user-attachments/assets/c586db75-984c-42db-9d4a-04292f1fd2c8)


2. Allow email communication only with approved domains
- Goto Google workspace Admin portal -> Apps -> Google Workspace -> Settings for Gmail -> Safety
- Enable 'Protect against domain spoofing based on similar domain names' option.
- This will protect against emails coming from domains that looks very similar to legit domains.
- Also Enable the option 'Protect against domain spoofing based on employee names'.
- As above this will stop email with similar employee names in the google workspace.
- We can choose action like Allow/Block/Quarantine.
- If we choose quarantine select which quarantine box either default/custom.
- Select the organization which we want to set this as active.

![image](https://github.com/user-attachments/assets/3b198631-cfbe-42d2-9910-3977bfbd1c32)

  
3. Restrict Drive file sharing to trusted domains
- Goto Google workspace Admin portal -> Apps -> Google Workspace -> Settings for Drive and Docs -> Sharing settings
- We can either turn OFF/ON the file sharing to every domains or select to sent emails to specific domain only. 
- Select 'Allowlisted domain' -> We should have predefind lists of domains which are whitelistes so the employess can send emails to -> Or we can create it from here.
- Also there options below for warning messages, Allowing incoming file shares and sharing to other non google users etc.
- Select organzation unit in which we want this to enroll.
  
![image](https://github.com/user-attachments/assets/f754ddfb-4a2b-4e10-8d7a-d57ff40fd728)



