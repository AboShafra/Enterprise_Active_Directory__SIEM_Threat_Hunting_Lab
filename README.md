# Enterprise Active Directory & SIEM Threat Hunting Lab

## Executive Summary
This project involved engineering a virtualized enterprise Active Directory environment integrated with a Splunk SIEM to simulate and detect real-world adversarial activity. The environment generated telemetry from controlled attacks, which was mapped to the MITRE ATT&CK framework to develop high-fidelity, threshold-based detection rules. The end-to-end SOC pipeline covers infrastructure deployment, log pipeline engineering, threat simulation, and automated response strategies.

---

## 🎯 Objectives
* Deploy a robust SOC log pipeline using Sysmon and Splunk Universal Forwarder.
* Simulate RDP brute-force attacks and persistence mechanisms.
* Achieve Common Information Model (CIM) compliance for normalized data analysis.
* Develop advanced SPL queries to detect threats with a Time to Detect (TTD) of ~2 minutes.

---

## 🛠️ Tools & Technologies
* **SIEM:** Splunk Enterprise (Ubuntu 22.04 LTS).
* **Endpoint Security:** Sysmon (Olaf Hartong Config), Windows Event Logs.
* **Adversarial Tools:** Kali Linux, Hydra, Atomic Red Team.
* **Infrastructure:** Windows Server 2022 (ADDS), Windows 10, VMware Workstation.

---

## 🏗️ Logical Architecture
The lab operated within a custom isolated virtual network (Vmnet7) to safely execute and monitor malicious activity.

* **Domain Controller:** 192.168.7.100 (Windows Server 2022)
* **Target Workstation:** 192.168.7.10 (Windows 10)
* **Splunk Indexer:** 192.168.7.132 (Ubuntu Server)
* **Attacker System:** 192.168.7.250 (Kali Linux)

---

## 🛡️ The SOC Pipeline: Data Engineering
To ensure high-visibility, I engineered a robust log ingestion pipeline:
1. **Telemetry Generation:** Deployed Sysmon across endpoints to capture process creation and network connections.
2. **Log Transport:** Configured Splunk Universal Forwarder to route logs over TCP port 9997.
3. **Normalization:** Installed the Splunk Add-on for Microsoft Windows to translate raw XML data into CIM-compliant fields.

---

## ⚔️ Attack Simulation & Detection Logic

### 1. RDP Brute Force (Credential Access)
* **Attack:** Targeted brute-force against the domain account `ibrahim` using Hydra.
* **Detection Logic:** Engineered an SPL query to detect high-volume authentication failures (EventCode 4625) within a 5-minute threshold.
* **Finding:** Identified the specific error code `0xc000006d` (bad authentication) originating from the attacker's IP.

### 2. Local Account Creation (Persistence)
* **Attack:** Automated simulation via Atomic Red Team to create a rogue administrator account.
* **Detection Logic:** Monitored Sysmon Event ID 1 (Process Creation) to hunt for `net.exe` command-line arguments.
* **Finding:** Extracted the exact command: `net user /add "T1136.001_Admin"` and identified the parent process as `cmd.exe`.

---

## 🚀 Incident Response Workflow
* **Triage:** Validated spikes in EventCode 4625 and confirmed sustained attempts from a single source.
* **Containment:** Blocked source IP (192.168.7.250) and disabled compromised accounts.
* **Recovery:** Reset credentials and audited systems for registry modifications.

---

## 🔧 Engineering Challenges
* **DNS Sinkholing:** Resolved installation failures for Atomic Red Team by hijacking local DNS via the Windows hosts file.
* **Time-Drift:** Fixed SIEM visibility issues caused by NTP desynchronization between the hypervisor and the VM.

---
*Developed by Ibrahim Alshami | 2026*
