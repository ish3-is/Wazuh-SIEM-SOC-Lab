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

## 📅 Detailed Timeline & Security Engineering Phases

### Phase 1: Environment Initialization & Initial Baselining (May 29 - 17:48)

The SIEM core was initialized, and log ingestion was verified across the environment by tracking initial high-volume authentication baselines coming into the manager.

<img width="802" height="686" alt="Initial Alert Hits Ingestion Dashboard" src="https://github.com/user-attachments/assets/f9007e26-6f5f-4c8d-b148-5f25ff710dbc" />

---

### Phase 2: Threat Simulation & Defenses Orchestration (May 29 - 18:14 to 18:42)

#### 1. Simulating the SSH Brute Force Attack Vector (18:14)
Launched a high-velocity SSH brute-force attack from the Kali Linux attacker platform using `Hydra` against the target Linux VM to generate anomalous authentication behavior and trigger rule correlations.

<img width="1280" height="1392" alt="Hydra Brute Force Attack Active" src="https://github.com/user-attachments/assets/83b7c935-372b-4714-87cb-41d38fce4877" />

#### 2. Accessing the Dev Tools / Console (18:39)
Utilized the Wazuh Dashboard's `Server Management -> Dev Tools` interface to analyze the API interaction possibilities before configuring the permanent configuration file.

<img width="1280" height="1392" alt="Wazuh API Console Access" src="https://github.com/user-attachments/assets/1c80fa39-76b6-4162-9e48-45bbfca5214c" />

#### 3. Navigating to XML Configuration Editor (18:42)
To implement permanent SOAR capabilities, the dashboard was used to access the manager's XML infrastructure configuration file (`ossec.conf`).

<img width="1280" height="1392" alt="Accessing Manager Management Configuration" src="https://github.com/user-attachments/assets/0bd4edc0-1dc9-49b7-890a-97b8ea8064dc" />

#### 4. Applying the Active Response (SOAR) Block (18:42)
The XML configuration was updated to introduce the automated mitigation payload. This instructs the manager to execute a `firewall-drop` script across the local endpoint whenever authentication failures match Rule ID `5712`.

`xml
<active-response>
  <command>firewall-drop</command>
  <location>local</location>
  <rules_id>5712</rules_id>
  <timeout>300</timeout>
</active-response>`
### Phase 3: Immediate Mitigation & Forensic Verification (May 29 - 18:42 to 18:51)
#### 1. Attacker Connection Dropped & Sluggish (18:42)
The Active Response trigger executed in real-time. On the Kali Linux side, the Hydra attack velocity immediately plummeted as connections were actively terminated, forcing a drop to only 48 tries/minute.

#### 2. Target Machine Local Firewall Proof (18:42)
Forensic proof was gathered on the target Ubuntu instance. Inspecting the local firewall (iptables) confirms that Wazuh's script dynamically pushed an explicit REJECT and DROP rule for the attacker IP address (192.168.1.5).

#### 3. Setup Validation: Cross-Platform Windows Log Feed (18:42 - 18:43)
To ensure complete visibility before analyzing Attack 1 and launching Attack 2, the Windows Server telemetry connection was validated by checking the active agent streaming status and aggregator feeds.

#### 4. Post-Attack SIEM Analytics Review (18:51)
Reviewed the comprehensive analytics dashboard mapping all concurrent authentication brute force triggers alongside global enterprise compliance baselines.

### Phase 4: Today's Insider Threat Scenario & Active Auditing (May 30 - 12:07)
Simulated an internal insider threat / lateral movement vector inside the Windows Server instance by forcing a rogue admin configuration update via an elevated PowerShell session:

`PowerShell
net user AttackerAdmin Password123! /add
net localgroup administrators AttackerAdmin /add
The Windows agent instantly intercepted the underlying security log architecture, firing high-severity indicators to the main management platform. Wazuh identified the specific identifiers (Rule 60109 for account creation, Rule 60154 for group escalation), resulting in a Critical Level 12 escalation alert.`

Furthermore, the platform seamlessly performed compliance correlation with NIST 800-53 controls AC.2 (Account Management) and IA.4 (Identifier Management).

### 🎯 Key Skills Demonstrated
SIEM/XDR Deployment & Engineering: Installed, registered, and maintained heterogeneous enterprise Wazuh agents.

Incident Response Automation (SOAR): Orchestrated Active Response rules to dynamically command network firewalls (iptables).

Cross-Platform Log Analysis: Extracted and parsed complex system logs including Windows Event IDs (4720, 4732) and Linux Syslogs.

Regulatory Compliance Framework Integration: Embedded operational activities within global security baselines like NIST 800-53.
