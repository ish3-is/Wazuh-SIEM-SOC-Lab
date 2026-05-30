# Building an Enterprise SOC Lab with Wazuh SIEM/XDR & Active Cyber Response

## 📌 Project Overview
Successfully engineered and deployed a fully functional, virtualized Security Operations Center (SOC) home lab. The primary objective was to configure a centralized **Wazuh SIEM/XDR** platform to ingest, correlate, and analyze multi-platform security event logs (Linux & Windows Server), simulate real-world cyber attacks, and orchestrate automated defensive mitigations (**Active Response**).

---

## 🛠️ Infrastructure & Lab Architecture
- **SIEM Core (Manager):** Wazuh OVA deployed on Oracle VirtualBox (Bridged Network configuration).
- **Linux Endpoint:** Ubuntu (SEED VM) equipped with a Wazuh Agent for SSH & authentication log auditing.
- **Windows Endpoint:** Windows Server 2019 Datacenter with Wazuh Agent for monitoring Active Directory/Local Accounts.
- **Attacker Platform:** Kali Linux utilizing automated credential-stuffing and discovery tools.

### Lab Status & Core Health Verification
Before launching threat simulations, the Wazuh Server API connection and cluster components were verified to ensure the environment was stable and operational.

<img width="1280" height="1392" alt="Wazuh API Status Verification" src="https://github.com/user-attachments/assets/0039d22d-0aa8-4e90-80c9-b5b57ad08c7f" />

<img width="1280" height="1392" alt="Wazuh Environment Infrastructure Verification" src="https://github.com/user-attachments/assets/a91c825d-7665-4ddc-8456-4e6c00dee437" />

---

## ⚔️ Simulated Use Cases & Incident Response Actions

### Case 1: Automated Mitigation of SSH Brute Force (Linux)

#### 1. The Attack & Threat Simulation
Executed a high-velocity SSH brute-force attack from Kali Linux using `Hydra` targeted at the Ubuntu SEED endpoint to flood the authentication logs.

#### 2. SIEM Detection & Mapping
Wazuh successfully captured over **2,273 security hits** in real-time. The dashboard aggregated the logs, mapping the activity to **NIST 800-53** controls (`AU.14`, `AC.7`) for authentication failures and unauthorized access attempts.

<img width="802" height="686" alt="Wazuh SSH Brute Force High Alert Hits" src="https://github.com/user-attachments/assets/f9007e26-6f5f-4c8d-b148-5f25ff710dbc" />

#### 3. Active Response Configuration (SOAR)
Modified the core manager infrastructure configuration (`ossec.conf`) via the environment control console to trigger an automated `firewall-drop` script the moment Rule ID `5712` is met.

<img width="1280" height="1392" alt="Accessing Manager Management Configuration" src="https://github.com/user-attachments/assets/1c80fa39-76b6-4162-9e48-45bbfca5214c" />

<img width="1280" height="1392" alt="Modifying Configuration Settings" src="https://github.com/user-attachments/assets/0bd4edc0-1dc9-49b7-890a-97b8ea8064dc" />

```xml
<active-response>
  <command>firewall-drop</command>
  <location>local</location>
  <rules_id>5712</rules_id>
  <timeout>300</timeout>
</active-response>
4. The Mitigation Result & Forensic Proof
The server updated the policy and instructed the endpoint's firewall (iptables) to seamlessly drop incoming attacker packets. The tool's velocity plummeted to just 48 tries/minute before timing out completely, proving a 100% automated mitigation success.

5. Local Firewall Verification on Target Machine
Verifying the target endpoint logs confirms that the local firewall rule was pushed by Wazuh, moving the attacker IP into a REJECT/DROP state.

Case 2: In-Depth Auditing of Privilege Escalation (Windows Server)
1. The Attack & Persistence Simulation
Simulated a credential theft/persistence mechanism by forcing a rogue administrator account creation directly via an elevated PowerShell session:

PowerShell
net user AttackerAdmin Password123! /add
net localgroup administrators AttackerAdmin /add
2. SIEM Detection & Log Analysis
The Wazuh Agent instantly intercepted the raw Windows Event Logs and escalated a Critical Level 12 Alert into the security events dashboard.

3. Rule Breakdown & Compliance Mapping
Rule ID 60109: User account created successfully (Event ID 4720).

Rule ID 60154: Administrators local group altered (Event ID 4732).

Compliance Mapping: Correlated directly with NIST 800-53 AC.2 (Account Management) and IA.4 (Identifier Management).

🎯 Key Skills Demonstrated
SIEM/XDR Deployment & Engineering: Installed, registered, and maintained heterogeneous enterprise Wazuh agents.

Incident Response Automation (SOAR): Orchestrated Active Response rules to dynamically command network firewalls (iptables).

Cross-Platform Log Analysis: Extracted and parsed complex system logs including Windows Event IDs (4720, 4732) and Linux Syslogs.

Regulatory Compliance Framework Integration: Embedded operational activities within global security baselines like NIST 800-53.
