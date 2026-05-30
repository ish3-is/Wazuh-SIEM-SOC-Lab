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
