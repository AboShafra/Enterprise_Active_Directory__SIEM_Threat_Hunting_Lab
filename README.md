# Enterprise Active Directory & SIEM Threat Hunting Lab

## 📝 Executive Summary
[cite_start]This project involved engineering a virtualized enterprise Active Directory environment integrated with a Splunk SIEM to simulate and detect real-world adversarial activity[cite: 3]. [cite_start]The environment generated telemetry from controlled attacks, which was mapped to the MITRE ATT&CK framework to develop high-fidelity, threshold-based detection rules[cite: 4]. [cite_start]The end-to-end SOC pipeline covers infrastructure deployment, log pipeline engineering, threat simulation, and automated response strategies[cite: 5].

---

## 🧠 Skills Learned
* [cite_start]**Lab Engineering & Virtualization:** Building an isolated enterprise network using VMware to safely execute adversarial simulations[cite: 23].
* [cite_start]**Log Pipeline Design:** Implementing end-to-end telemetry ingestion using Sysmon and Splunk Universal Forwarders[cite: 53, 55].
* [cite_start]**Data Normalization & CIM Compliance:** Mastering Splunk Add-ons to parse raw XML event data into Common Information Model (CIM) compliant fields[cite: 58].
* [cite_start]**Detection Engineering:** Developing custom, threshold-based SPL queries to identify anomalies and high-volume authentication failures[cite: 70].
* [cite_start]**Threat Hunting & Analysis:** Correlating Windows Event IDs with Sysmon process lineage to identify persistence and privilege escalation[cite: 89, 96].
* [cite_start]**Incident Response & Triage:** Formulating response workflows including containment, account disablement, and source IP blocking[cite: 100, 115].
* [cite_start]**Technical Troubleshooting:** Resolving complex engineering roadblocks such as DNS interception and SIEM time-drift[cite: 132, 138].

---

## 🛠️ Tools & Technologies
* [cite_start]**Hypervisor:** VMware Workstation (Isolated Vmnet7 network)[cite: 41, 42].
* [cite_start]**SIEM / Indexer:** Splunk Enterprise (Running on Ubuntu Server 22.04 LTS)[cite: 45].
* [cite_start]**Domain Controller:** Windows Server 2022 (Active Directory Domain Services)[cite: 46].
* [cite_start]**Endpoint Telemetry:** Sysmon (Olaf Hartong's configuration) & Windows Security Logs[cite: 53, 55].
* [cite_start]**Adversarial Frameworks:** Kali Linux, Hydra (Brute-force), and Atomic Red Team[cite: 48, 65, 84].

---

## 🏗️ Logical Architecture
[cite_start]The lab operated within a custom isolated virtual network (192.168.7.0/24) to safely monitor malicious activity[cite: 25].

* [cite_start]**Domain Controller (ADDC01):** 192.168.7.100 [cite: 46]
* [cite_start]**Target Workstation (Windows 10):** 192.168.7.10 [cite: 47]
* [cite_start]**Splunk Server:** 192.168.7.132 [cite: 45]
* [cite_start]**Attacker System (Kali Linux):** 192.168.7.250 [cite: 48]

---

## 🛡️ The SOC Pipeline: Data Engineering
[cite_start]To ensure high-visibility threat hunting, I engineered a robust log ingestion pipeline[cite: 50]:
1. [cite_start]**Telemetry Generation:** Deployed Sysmon across endpoints to capture granular process creation and network connections[cite: 53].
2. [cite_start]**Log Transport:** Configured Splunk Universal Forwarders to route logs securely over TCP port 9997[cite: 55].
3. [cite_start]**Normalization:** Operationalized the Splunk Add-on for Microsoft Windows to ensure CIM compliance[cite: 57, 58].

---

## ⚔️ Attack Simulation & Detection Logic

### 1. RDP Brute Force (Credential Access - T1110.001)
* [cite_start]**Attack:** Targeted RDP brute-force against the domain account `ibrahim` using Hydra[cite: 65, 68].
* [cite_start]**Detection Logic:** Engineered an SPL query to detect high-volume authentication failures (EventCode 4625) within a 5-minute threshold[cite: 70, 71].
* [cite_start]**Findings:** The SIEM successfully correlated error code `0xc000006d` (bad authentication) originating from the attacker's IP 192.168.7.250[cite: 76, 79].

### 2. Local Account Creation (Persistence - T1136.001)
* [cite_start]**Attack:** Automated simulation via Atomic Red Team to create rogue administrator accounts[cite: 84].
* [cite_start]**Detection Logic:** Monitored Sysmon Event ID 1 (Process Creation) to hunt for `net.exe` command-line arguments[cite: 89, 92].
* [cite_start]**Findings:** Extracted exact command-line arguments: `net user /add "T1136.001_Admin"` and identified `cmd.exe` as the parent process[cite: 94, 96].

---

## 🚀 Incident Response & Performance
* [cite_start]**Triage:** Validated spikes in EventCode 4625 and confirmed sustained, repeated attempts from a single source[cite: 106, 109].
* [cite_start]**Containment:** Formulated strategies to block source IP 192.168.7.250 and disable compromised accounts[cite: 115, 116].
* [cite_start]**Performance Metric:** Achieved a **Time to Detect (TTD) of ~2 minutes**[cite: 143].

---

## 🔧 Engineering Challenges
* [cite_start]**Transparent DNS Sinkholing:** Resolved installation failures for Atomic Red Team by hardcoding authoritative IPs into the Windows hosts file[cite: 134, 136].
* [cite_start]**SIEM Pipeline Time-Drift:** Fixed visibility issues caused by NTP desynchronization between the hypervisor and the domain clocks[cite: 139, 141].

---
*Developed by Ibrahim Abdulsalam Alshami | 2026*
