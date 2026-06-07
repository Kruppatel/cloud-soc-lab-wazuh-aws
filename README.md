# Cloud SOC Lab — Threat Detection & Incident Response using Wazuh SIEM on AWS

## Project Overview
Built a fully functional cloud-based Security Operations Center (SOC) lab on AWS to simulate real-world cyber attacks and detect them using Wazuh SIEM. This project demonstrates hands-on experience with threat detection, incident response, and compliance monitoring in a hybrid cloud environment.

## Architecture

```
Local Mac (UTM)
├── Kali Linux VM (192.168.64.3)     — Attacker machine
└── Windows 11 VM (192.168.64.10)   — Victim machine with Wazuh Agent

AWS Cloud
└── Ubuntu EC2 t3.medium
    ├── Wazuh Manager     — Receives and analyzes logs
    ├── Wazuh Dashboard   — Visualizes alerts
    ├── CloudTrail        — AWS API monitoring
    └── GuardDuty         — Cloud threat detection
```

## Tools & Technologies
- **SIEM:** Wazuh 4.7.5
- **Cloud:** AWS EC2 (Ubuntu 26.04)
- **Attack Machine:** Kali Linux
- **Victim Machine:** Windows 11 Pro
- **Attack Tools:** Nmap, Hydra, CrackMapExec

## Attack Simulations

### 1. Network Reconnaissance (Nmap)
- **Tool:** Nmap
- **Command:** `nmap -sV -sC 192.168.64.10`
- **Finding:** Discovered open ports 135, 139, 445 (SMB)
- **MITRE ATT&CK:** T1046 — Network Service Discovery
- **Wazuh Detection:** 542 security alerts generated

### 2. SSH Brute Force (Hydra)
- **Tool:** Hydra
- **Command:** `hydra -l administrator -P rockyou.txt 192.168.64.10 ssh`
- **Finding:** 20 authentication failure alerts generated
- **MITRE ATT&CK:** T1110 — Brute Force
- **Wazuh Detection:** Authentication failure alerts, rule 60122

### 3. SMB Brute Force (CrackMapExec)
- **Tool:** CrackMapExec
- **Finding:** Successfully fingerprinted target — Windows 11, SMB signing enabled
- **MITRE ATT&CK:** T1021 — Remote Services
- **Result:** Windows Firewall blocked connections — documented as expected behavior

## Detections & Compliance Mapping
Every alert was automatically mapped to multiple compliance frameworks by Wazuh:

| Framework | Control |
|---|---|
| NIST 800-53 | AC.7, AU.14 |
| GDPR | IV_32.2 |
| HIPAA | 164.312.b |
| PCI-DSS | 10.2.5 |

## Key Findings
- 599 total security events detected
- 20 authentication failures from brute force attack
- MITRE ATT&CK techniques detected: Brute Force, Valid Accounts, Lateral Movement, Credential Access, Defense Evasion
- All attacks detected and logged in real time

## Screenshots

| Screenshot | Description |
|---|---|
| `ec2-wazuh-server-running.png` | AWS EC2 instance running |
| `wazuh-windows-agent-connected.png` | Windows 11 agent connected to Wazuh |
| `wazuh-security-events-alerts.png` | 542 security alerts on dashboard |
| `wazuh-brute-force-detected.png` | Brute force attack detected |
| `wazuh-event-detail-forensics.png` | Detailed forensic event analysis |
| `wazuh-compliance-frameworks.png` | Compliance framework mappings |
| `wazuh-mitre-attack-full-dashboard.png` | Full MITRE ATT&CK dashboard |
| `nmap-scan-results.png` | Nmap reconnaissance results |

## Author
Krup Patel — SOC Analyst
