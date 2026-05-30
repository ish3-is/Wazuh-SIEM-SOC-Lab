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

## 📅 Timeline & Step-by-Step Security Engineering

### Phase 1: Initial Ingestion & Log Correlation (May 29 - 17:48)
The environment was initialized, and log ingestion was verified by tracking initial authentication baselines and high-volume triggers coming into the manager.

<img width="802" height="686" alt="Initial Alert Hits Ingestion" src="https://github.com/user-attachments/assets/f9007e26-6f5f-4c8d-b148-5f25ff710dbc" />

---

### Phase 2: Active Response Configuration & Engineering (May 29 - 18:14 to 18:42)

#### 1. Simulating the Brute Force Attack Vector (18:14)
Launched a high-velocity SSH brute-force attack from the Kali Linux attacker platform using `Hydra` against the target Linux VM to generate anomalous authentication behavior.

<img width="1280" height="1392" alt="Hydra Simulation Active" src="https://github.com/user-attachments/assets/83b7c935-372b-4714-87cb-41d38fce4877" />

#### 2. Accessing and Modifying the Server Configuration (18:39 - 18:42)
Navigated through the Wazuh Dashboard's `Server Management` settings to access the core XML infrastructure configuration (`ossec.conf`).

<img width="1280" height="1392" alt="Navigating Management Controls" src="https://github.com/user-attachments/assets/1c80fa39-76b6-4162-9e48-45bbfca5214c" />

Opened the XML configuration editor to introduce an automated mitigation block:

<img width="1280" height="1392" alt="Opening Configuration Editor" src="https://github.com/user-attachments/assets/0bd4edc0-1dc9-49b7-890a-97b8ea8064dc" />

Appended the SOAR `active-response` block to dynamically trigger a `firewall-drop` payload across the local host whenever authentication failures meet Rule ID `5712`:

```xml
<active-response>
  <command>firewall-drop</command>
  <location>local</location>
  <rules_id>5712</rules_id>
  <timeout>300</timeout>
</active-response>
Phase 3: Automated Mitigation & Forensic Verification (May 29 - 18:42 to 18:51)
1. Packet Interception & Threat Choking (18:42)
Once the manager process restarted, the automated pipeline executed. The host's local firewall immediately dropped attacker packets, forcing Hydra connection timeouts and reducing its processing capability down to a stagnant 48 tries/minute.

2. Local Endpoint Firewall Rules Inspection (18:42)
Inspecting the target Linux instance (iptables) confirmed that Wazuh's script dynamically pushed an explicit REJECT and DROP rule for the attacker IP address (192.168.1.5).

3. Analyzing the Active Windows Logs Connection (18:42 - 18:43)
Verified endpoint communication and active telemetry streaming from the infrastructure to confirm complete network-wide visibility.

4. Full Threat Ingestion Dashboard Review (18:51)
Reviewed the comprehensive analytics dashboard mapping all concurrent brute force triggers alongside global enterprise compliance baselines.

Phase 4: Windows Server Auditing & Privilege Escalation Detection (May 30 - 12:07)
Simulated an internal insider threat / lateral movement vector inside the Windows Server instance by forcing a rogue admin configuration update via PowerShell.

PowerShell
net user AttackerAdmin Password123! /add
net localgroup administrators AttackerAdmin /add
The Windows agent instantly intercepted the underlying security log architecture, firing high-severity indicators to the main management platform:

Rule ID 60109 (Event ID 4720): New User Account Created.

Rule ID 60154 (Event ID 4732): Local Administrators Group Membership Escalated.

The SIEM parsed the Windows structural attributes and seamlessly performed compliance correlation with NIST 800-53 controls AC.2 (Account Management) and IA.4 (Identifier Management).

🎯 Key Skills Demonstrated
SIEM/XDR Deployment & Engineering: Installed, registered, and maintained heterogeneous enterprise Wazuh agents.

Incident Response Automation (SOAR): Orchestrated Active Response rules to dynamically command network firewalls (iptables).

Cross-Platform Log Analysis: Extracted and parsed complex system logs including Windows Event IDs (4720, 4732) and Linux Syslogs.

Regulatory Compliance Framework Integration: Embedded operational activities within global security baselines like NIST 800-53.
