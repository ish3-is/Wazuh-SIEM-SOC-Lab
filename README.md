<img width="1280" height="1392" alt="لقطة شاشة 2026-05-29 184216" src="https://github.com/user-attachments/assets/eedf1155-cfcd-4fdf-8129-03f588e6a3d7" />
<img width="1280" height="1392" alt="لقطة شاشة 2026-05-29 184240" src="https://github.com/user-attachments/assets/b93d0e12-ad3f-4dc8-b885-3970f98242e8" />
<img width="802" height="686" alt="لقطة شاشة 2026-05-29 184245" src="https://github.com/user-attachments/assets/90b9d95f-8cf6-43fe-9c2a-cc6ee9ecea4b" />
<img width="1280" height="1392" alt="لقطة شاشة 2026-05-29 184259" src="https://github.com/user-attachments/assets/a4ae83fe-aca1-4e4b-bbb0-d042fc2425ae" />
<img width="1280" height="1392" alt="لقطة شاشة 2026-05-29 184308" src="https://github.com/user-attachments/assets/ddc5b409-83a7-44ba-8b3e-b884f062f0dd" />
<img width="1280" height="1392" alt="لقطة شاشة 2026-05-29 184318" src="https://github.com/user-attachments/assets/01d244ed-44c5-4f3a-ab1e-449192b467d2" />
<img width="1280" height="1392" alt="لقطة شاشة 2026-05-29 184323" src="https://github.com/user-attachments/assets/cf0d15fe-98d3-483a-aef5-3d66a0580887" />
<img width="2555" height="1439" alt="لقطة شاشة 2026-05-29 185104" src="https://github.com/user-attachments/assets/aa89b9a9-df60-4ebd-ac24-e74c97900b48" />
<img width="1280" height="1392" alt="لقطة شاشة 2026-05-30 120721" src="https://github.com/user-attachments/assets/90865612-d3cc-47b7-b358-6afcf39fefc1" />
<img width="1280" height="1392" alt="لقطة شاشة 2026-05-30 120738" src="https://github.com/user-attachments/assets/4477af52-f96a-4e64-97e1-da78efccc5cf" />
<img width="1280" height="1392" alt="لقطة شاشة 2026-05-29 184204" src="https://github.com/user-attachments/assets/5dba9513-c664-4d65-afce-69e0eeda9069" />
# Building an Enterprise SOC Lab with Wazuh SIEM/XDR & Active Cyber Response

## 📌 Project Overview
Successfully engineered and deployed a fully functional, virtualized Security Operations Center (SOC) home lab. The primary objective was to configure a centralized **Wazuh SIEM/XDR** platform to ingest, correlate, and analyze multi-platform security event logs (Linux & Windows Server), simulate real-world cyber attacks, and orchestrate automated defensive mitigations (**Active Response**).

---

## 🛠️ Infrastructure & Lab Architecture
- **SIEM Core (Manager):** Wazuh OVA deployed on Oracle VirtualBox (Bridged Network configuration).
- **Linux Endpoint:** Ubuntu (SEED VM) equipped with a Wazuh Agent for SSH & authentication log auditing.
- **Windows Endpoint:** Windows Server 2019 Datacenter with Wazuh Agent for monitoring Active Directory/Local Accounts.
- **Attacker Platform:** Kali Linux utilizing automated credential-stuffing and discovery tools.

---

## ⚔️ Simulated Use Cases & Incident Response Actions

### Case 1: Automated Mitigation of SSH Brute Force (Linux)
1. **The Attack:** Executed a high-velocity SSH brute-force attack from Kali Linux using `Hydra`.
2. **SIEM Detection:** Wazuh successfully captured over **2,273 security hits** in real-time, matching the activity to **NIST 800-53** controls (`AU.14`, `AC.7`) for authentication failures.
3. **Active Response Action:** Modified the core server infrastructure configuration (`ossec.conf`) to deploy an automated `firewall-drop` script when Rule ID `5712` is triggered.
4. **The Result:** The firewall seamlessly dropped incoming attacker packets. The tool's velocity plummeted to just **48 tries/minute** before timing out completely, proving a 100% mitigation success.

### Case 2: In-Depth Auditing of Privilege Escalation (Windows Server)
1. **The Attack:** Simulated a persistence mechanism by creating a rogue administrator account via PowerShell:
   ```powershell
   net user AttackerAdmin Password123! /add
   net localgroup administrators AttackerAdmin /add
   SIEM Detection: The Wazuh Agent instantly intercepted the event logs and escalated a Critical Level 12 Alert:

Rule ID 60109: User account created (Event ID 4720).

Rule ID 60154: Administrators local group altered (Event ID 4732).

Compliance Mapping: Correlated directly with NIST 800-53 AC.2 (Account Management) and IA.4 (Identifier Management).

🎯 Key Skills Demonstrated
SIEM/XDR Deployment & Engineering (Wazuh configuration, Agent registration).

Incident Response Automation (Orchestrating Active Response & firewall drops).

Cross-Platform Log Analysis (Windows Event Logs Event ID 4720/4732 & Linux Auth logs).

Regulatory Compliance Mapping (NIST 800-53 Framework integration).
