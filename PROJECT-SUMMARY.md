# Project Summary — Cloud SOC Lab

## Project Name
Cloud SOC Lab — Threat Detection & Incident Response using Wazuh SIEM on AWS

## Objective
Deploy a cloud-based SOC environment on AWS, simulate real-world attacks from Kali Linux, and detect them using Wazuh SIEM with full MITRE ATT&CK and compliance mapping.

## Environment
| Component | Details |
|---|---|
| Cloud Platform | AWS EC2 (us-east-2, Ohio) |
| SIEM | Wazuh 4.7.5 |
| Server OS | Ubuntu 26.04 LTS |
| Attacker | Kali Linux (UTM on Apple Silicon Mac) |
| Victim | Windows 11 Pro (UTM on Apple Silicon Mac) |
| Instance Type | t3.medium (4GB RAM, 2 vCPU) |

## Attacks Simulated
| Attack | Tool | MITRE ID | Result |
|---|---|---|---|
| Network Reconnaissance | Nmap | T1046 | 542 alerts generated |
| SSH Brute Force | Hydra | T1110 | 20 auth failures detected |
| SMB Brute Force | CrackMapExec | T1021 | Target fingerprinted |

## Key Results
- 599 total security events detected
- 20 brute force attempts detected in real time
- MITRE ATT&CK techniques automatically mapped
- Compliance frameworks mapped: NIST 800-53, GDPR, HIPAA, PCI-DSS

## Skills Demonstrated
- Cloud infrastructure deployment (AWS EC2, VPC, Security Groups)
- SIEM deployment and configuration (Wazuh)
- Threat detection and alert analysis
- Attack simulation (Nmap, Hydra, CrackMapExec)
- MITRE ATT&CK framework mapping
- Compliance monitoring (NIST, GDPR, HIPAA, PCI-DSS)
- Incident response documentation
- Linux administration (Ubuntu, SSH, systemctl)
