# Active Directory Breach Investigation  
### EmberForge Source Code Leak

---

## Overview

This project analyzes a full enterprise breach that resulted in the theft of proprietary source code from EmberForge Studios.

The attacker progressed from initial access to full domain compromise, leveraging credential dumping, lateral movement, and cloud-based data exfiltration.

---

## Executive Summary

An attacker compromised a workstation via malicious DLL execution and rapidly escalated privileges, ultimately gaining full control of the domain.

**Key attacker actions:**

- UAC bypass using `fodhelper.exe`  
- Credential theft via LSASS dumping and `ntds.dit` extraction  
- Lateral movement using SMB and remote services  
- Data exfiltration via `rclone` to MEGA cloud storage  
- Persistence via scheduled tasks and a backdoor admin account  
- Log clearing using `wevtutil` to evade detection  

---

## Initial Access

User executed a malicious DLL (`review.dll`) from a mounted disk, bypassing security controls.

<br>

<img width="632" height="222" alt="Screenshot 2026-04-18 at 8 15 21 AM" src="https://github.com/user-attachments/assets/f6c7a362-9279-44e4-8d81-398e9af9b5be" />

---

## Privilege Escalation

UAC bypass performed using `fodhelper.exe` with registry manipulation.

<br>

<img width="630" height="173" alt="Screenshot 2026-04-18 at 8 16 59 AM" src="https://github.com/user-attachments/assets/0f283c0e-c46a-4610-b007-add458543844" />

---

## Command & Control

Malware (`update.exe`) established communication with attacker-controlled infrastructure.

- Domain: `cdn.cloud-endpoint.net`  
- IP: `104.21.30.237`  

<br>

<img width="560" height="203" alt="Screenshot 2026-04-18 at 8 17 59 AM" src="https://github.com/user-attachments/assets/3174150d-ce36-4a45-b74f-ba30af454389" />

---

## Lateral Movement

The attacker moved across systems using SMB and remote service execution.

<br>

<img width="617" height="170" alt="Screenshot 2026-04-18 at 8 19 02 AM" src="https://github.com/user-attachments/assets/3a75d300-3e42-4f26-9822-9b966bb8adcb" />

---

## Credential Access

Credentials were extracted from both endpoint memory and Active Directory:

- LSASS dump  
- `ntds.dit` (Domain Controller compromise)

<br>

<img width="626" height="199" alt="Screenshot 2026-04-18 at 8 19 52 AM" src="https://github.com/user-attachments/assets/eb957f57-5686-4d7d-b565-d23c5871f3c5" />

---

## Data Exfiltration

Sensitive data was compressed and exfiltrated using `rclone` to MEGA.

<br>

<img width="630" height="166" alt="Screenshot 2026-04-18 at 8 20 50 AM" src="https://github.com/user-attachments/assets/8a5e1c10-977e-4c43-b063-9ab9597ad18c" />

---

## Domain Compromise

The attacker achieved persistence and full domain control:

- Created `svc_backup` (Domain Admin)  
- Installed scheduled task for persistence  

<br>

<img width="630" height="204" alt="Screenshot 2026-04-18 at 8 21 41 AM" src="https://github.com/user-attachments/assets/784b1983-e0d5-43f3-bfdf-35b7ac3af081" />

---

## Defense Evasion

Logs were cleared using `wevtutil`, reducing forensic visibility.

<br>

<img width="626" height="138" alt="Screenshot 2026-04-18 at 8 22 50 AM" src="https://github.com/user-attachments/assets/3ff34e9a-956e-45b9-ac8a-c66a822b97be" />

---

## MITRE ATT&CK Mapping

| Tactic | Technique |
|--------|----------|
| Initial Access | T1204 – User Execution |
| Privilege Escalation | T1548 – UAC Bypass |
| Credential Access | T1003 – Credential Dumping |
| Lateral Movement | T1021 – SMB |
| Exfiltration | T1048 – Exfiltration Over Web |
| Defense Evasion | T1070 – Indicator Removal |

---

## Indicators of Compromise

**IPs**
- 104.21.30.237  
- 66.203.125.15  

**Domains**
- cdn.cloud-endpoint.net  

**Files**
- review.dll  
- update.exe  
- ntds.dit  

---

## Analyst Insight

This attack demonstrates how quickly a single user execution event can escalate into full domain compromise when identity protections and monitoring are insufficient.

The attacker relied heavily on built-in system tools, blending malicious activity with legitimate processes to evade detection.

---

## 👤 Author

**Jason Stokes**  
Cybersecurity | Threat Hunting | Incident Response
