# Performing-Root-Cause-Analysis

# 🔍 Assisted Lab: Performing Root Cause Analysis

## 📌 Overview

This lab demonstrates the process of performing **Root Cause Analysis (RCA)** after a simulated security breach. Using **Wazuh SIEM**, **Windows Event Viewer**, **OPNsense Firewall**, **Wireshark**, and Windows security logs, the investigation traces attacker activity from the initial alert through evidence collection to identifying the attacker and determining the attack path.

The investigation follows a realistic incident response workflow used by Security Operations Center (SOC) analysts.

---

## 🎯 Objectives

This lab aligns with the following **CompTIA Security+ (SY0-701)** objectives:

- **2.2** – Explain common threat vectors and attack surfaces
- **2.4** – Analyze indicators of malicious activity
- **4.4** – Explain security alerting and monitoring concepts and tools
- **4.8** – Explain appropriate incident response activities
- **4.9** – Use data sources to support an investigation

---

# 🖥️ Lab Environment

| System | Purpose |
|---------|----------|
| Kali Linux | Investigation workstation |
| Wazuh SIEM | Security monitoring and alert analysis |
| DC10 | Windows Server 2019 Domain Controller |
| MS10 | Windows Server 2016 |
| PC10 | Windows Server 2019 Client |
| ROUTER-BORDER | OPNsense Firewall |
| Firefox | Web investigation |
| Event Viewer | Windows log analysis |
| Wireshark | Packet capture analysis |

---

# 🛠️ Technologies Used

- Wazuh SIEM
- Windows Event Viewer
- Windows Audit Policies
- Windows Security Logs
- OPNsense Firewall
- Wireshark
- Firefox
- PowerShell
- Batch Scripts
- HTTP Traffic Analysis
- RDP
- Proxy Configuration
- Root Cause Analysis (RCA)

---

# 📚 Skills Learned

- Security event investigation
- SIEM alert analysis
- Root cause analysis
- Event log correlation
- Windows auditing
- RDP investigation
- Security timeline reconstruction
- Log correlation across multiple systems
- Firewall log analysis
- Packet capture analysis
- Social engineering investigation
- Phishing analysis
- Credential theft investigation
- Evidence collection
- Incident response methodology

---

# 🚨 Incident Summary

The Security Operations Center (SOC) generated an alert indicating that Windows auditing had been disabled on the Domain Controller (DC10).

Because disabling auditing is a common attacker technique used to hide malicious activity, an investigation was initiated to determine:

- Who performed the changes
- How the attacker obtained access
- What systems were involved
- The complete attack timeline

---

# 🔎 Investigation Process

## Phase 1 – Wazuh Security Alerts

The investigation began inside **Wazuh Security Events**.

Actions performed:

- Reviewed Rule ID **60112**
- Identified multiple audit policy changes
- Examined usernames responsible
- Located associated Event Record IDs
- Reviewed Rule ID **92653**
- Identified suspicious Remote Desktop activity

Evidence discovered:

- Audit policies were disabled
- Administrator account **jaime** performed the changes
- Connection originated through **Remote Desktop (RDP)**
- Source system was **MS10**
- Activity was inconsistent with normal administrator behavior

---

## Phase 2 – DC10 Investigation

Using Windows Event Viewer:

- Reviewed Security Log
- Located Event ID **4719**
- Confirmed audit policy modifications
- Located RDP login event
- Verified Logon Type **10 (Remote Interactive)**

Findings:

- Windows auditing had been disabled
- RDP session originated from MS10
- Timeline matched SIEM alerts

---

## Phase 3 – MS10 Investigation

Windows Security logs showed:

- User **dylan** logged directly into MS10
- Dylan initiated the RDP session
- Jaime's credentials were used to access DC10
- Dylan physically entered the data center

Evidence confirmed:

- Dylan—not Jaime—initiated the attack.

---

## Phase 4 – PC10 Investigation

Jaime's workstation was examined.

Investigation included:

- Thunderbird email
- Downloaded files
- Firefox settings
- Proxy configuration

Findings:

- Jaime received a phishing email.
- The email encouraged downloading **proxyset.bat**.
- The batch script modified Firefox proxy settings.
- Firefox traffic was redirected through **MS10**.

This established the first stage of the compromise.

---

## Phase 5 – Firewall Investigation

Using OPNsense Firewall logs:

The investigation verified:

- Traffic from PC10
- Proxy communications
- Destination IP
- Destination port
- Timeline correlation

Evidence showed:

- Browser traffic flowed through MS10.
- Communication to Juice Shop occurred.
- HTTP traffic was unencrypted.
- MS10 acted as a malicious proxy.

---

## Phase 6 – Wireshark Investigation

Wireshark packet capture analysis revealed:

- HTTP POST requests
- Login credentials transmitted in plaintext
- Username
- Password

Evidence demonstrated:

- Jaime's credentials were captured during an unencrypted web session.
- Dylan later used these credentials to access DC10.

This completed the root cause analysis.

---

# 🧩 Attack Timeline

1. Dylan logged into MS10.
2. Dylan sent a phishing email to Jaime.
3. Jaime downloaded and executed **proxyset.bat**.
4. Firefox proxy settings were modified.
5. Jaime visited the Juice Shop website.
6. Browser traffic passed through MS10.
7. Credentials were captured using packet sniffing.
8. Dylan used Jaime's credentials.
9. Dylan connected to DC10 via RDP.
10. Windows auditing was disabled.
11. Wazuh detected the audit policy changes.
12. SOC initiated an investigation.

---

# 🔐 Indicators of Compromise (IOCs)

- Audit policies disabled
- Event ID 4719
- Rule ID 60112
- Rule ID 92653
- Unauthorized RDP session
- Suspicious proxy configuration
- proxyset.bat
- proxy.ps1
- Plaintext HTTP traffic
- Captured credentials
- Unusual administrator activity
- Data center access by unauthorized employee

---

# ⚠️ Root Cause

The attack succeeded because of multiple security weaknesses:

- Successful phishing attack
- User executed a malicious script
- Proxy settings were modified
- HTTP traffic was transmitted without encryption
- Credentials were intercepted
- Stolen administrator credentials were reused
- Audit logging was disabled to hide attacker activity

---

# 🛡️ Security Recommendations

- Improve phishing awareness training.
- Implement application allow-listing.
- Block unauthorized script execution.
- Prevent users from modifying proxy settings.
- Require HTTPS for all web applications.
- Restrict physical access to data centers.
- Prevent standard users from logging into servers.
- Restrict RDP usage for administrators.
- Enforce credential management best practices.
- Implement multi-factor authentication (MFA).
- Increase monitoring of privileged accounts.
- Enable centralized logging and SIEM alerting.
- Regularly review Windows audit policies.
- Continuously monitor Remote Desktop activity.

---

# 📖 Key Takeaways

This lab demonstrated how a Security Analyst performs a complete root cause investigation by correlating evidence from multiple sources.

Throughout the investigation, logs from Wazuh, Windows Event Viewer, OPNsense Firewall, and Wireshark were combined to reconstruct the attack timeline and identify the attacker.

The exercise highlights the importance of:

- SIEM monitoring
- Log correlation
- Windows event analysis
- Network traffic analysis
- Incident response procedures
- Digital forensics
- Security monitoring
- Root Cause Analysis (RCA)

---

# 🏷️ Tags

`Security+` `CompTIA` `Root Cause Analysis` `Incident Response` `Digital Forensics` `SOC Analyst` `Wazuh` `SIEM` `Windows Security` `Event Viewer` `Wireshark` `Firewall` `OPNsense` `RDP` `Phishing` `Credential Theft` `Packet Analysis` `Threat Hunting` `Cybersecurity Lab`
