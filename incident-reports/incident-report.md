# Incident Report — Brute Force Attack & Network Reconnaissance

**Report ID:** IR-2026-001  
**Date:** June 7, 2026  
**Analyst:** Krup Patel  
**Severity:** Medium  
**Status:** Resolved  

---

## 1. Executive Summary

On June 7, 2026, the Wazuh SIEM detected a series of malicious activities targeting the Windows 11 endpoint (windows-victim, 192.168.64.10). The attack included network reconnaissance via port scanning followed by brute force authentication attempts against SSH and SMB services. All attacks were detected in real time by Wazuh SIEM deployed on AWS. No unauthorized access was achieved. The attacker was blocked by Windows Firewall and SSH rate limiting.

---

## 2. Incident Timeline

| Time (UTC) | Event |
|---|---|
| 17:06 | Attacker (192.168.64.3) initiated Nmap port scan against 192.168.64.10 |
| 17:06 | Wazuh detected 542 security events from reconnaissance activity |
| 17:14 | Hydra SSH brute force initiated against port 22 |
| 17:14 | Wazuh detected first authentication failure (rule 60122) |
| 17:22 | Second wave of SSH brute force attempts |
| 17:23 | 20 authentication failure alerts generated in Wazuh |
| 17:23 | Windows rate limiting blocked further attempts |
| 17:14 | CrackMapExec SMB brute force attempted against port 445 |
| 17:14 | Windows Firewall blocked NETBIOS connections |
| 17:25 | Total 599 security events logged, incident contained |

---

## 3. Attack Details

### 3.1 Network Reconnaissance

- **Tool:** Nmap 7.98
- **Command:** `nmap -sV -sC 192.168.64.10`
- **Source IP:** 192.168.64.3 (Kali Linux)
- **Target IP:** 192.168.64.10 (Windows 11)
- **Open Ports Discovered:**
  - 135/tcp — Microsoft Windows RPC
  - 139/tcp — Microsoft Windows NetBIOS
  - 445/tcp — SMB
- **OS Fingerprint:** Windows 11 / Server 2025
- **Hostname:** WIN-B4NBUJUI7M6
- **MITRE ATT&CK:** T1046 — Network Service Discovery

### 3.2 SSH Brute Force

- **Tool:** Hydra v9.6
- **Target:** ssh://192.168.64.10:22
- **Username:** administrator
- **Wordlist:** rockyou.txt (14,344,399 passwords)
- **Result:** All attempts failed — Windows blocked after threshold
- **Alerts Generated:** 20 authentication failure events
- **Wazuh Rule:** 60122, Level 5
- **MITRE ATT&CK:** T1110 — Brute Force

### 3.3 SMB Brute Force

- **Tool:** CrackMapExec
- **Target:** smb://192.168.64.10:445
- **Username:** administrator
- **Result:** Windows Firewall blocked NETBIOS connections
- **Intelligence Gathered:** SMB signing enabled, SMBv1 disabled
- **MITRE ATT&CK:** T1021 — Remote Services

---

## 4. MITRE ATT&CK Mapping

| Tactic | Technique | ID | Tool | Detected |
|---|---|---|---|---|
| Reconnaissance | Network Service Discovery | T1046 | Nmap | ✅ |
| Credential Access | Brute Force | T1110 | Hydra | ✅ |
| Lateral Movement | Remote Services | T1021 | CrackMapExec | ✅ |
| Defense Evasion | Valid Accounts | T1078 | — | ✅ |
| Persistence | Valid Accounts | T1078 | — | ✅ |

---

## 5. Affected Systems

| System | IP | Role | Impact |
|---|---|---|---|
| windows-victim | 192.168.64.10 | Windows 11 Endpoint | Targeted — No breach |
| wazuh-server | 172.31.39.137 | AWS SIEM | Monitoring only |

---

## 6. Detection Summary

| Metric | Value |
|---|---|
| Total alerts generated | 599 |
| Authentication failures | 20 |
| Authentication successes | 52 (legitimate) |
| Highest alert level | 5 |
| Detection time | Real time |
| Time to contain | Automatic (Windows Firewall) |

---

## 7. Compliance Impact

| Framework | Control | Description |
|---|---|---|
| NIST 800-53 | AC.7 | Unsuccessful logon attempts |
| NIST 800-53 | AU.14 | Session audit |
| GDPR | IV_32.2 | Security of processing |
| HIPAA | 164.312.b | Audit controls |
| PCI-DSS | 10.2.5 | Invalid logical access attempts |

---

## 8. Root Cause Analysis

The attack originated from an internal network attacker (Kali Linux VM on the same UTM network). The attacker performed systematic reconnaissance followed by credential brute forcing. No vulnerability was exploited — the attacker relied entirely on common password guessing which was blocked by Windows built-in security controls.

---

## 9. Response Actions Taken

| Action | Status |
|---|---|
| Identified attacker IP (192.168.64.3) | ✅ Complete |
| Confirmed no unauthorized access | ✅ Complete |
| Reviewed all 599 alerts in Wazuh | ✅ Complete |
| Documented MITRE ATT&CK mapping | ✅ Complete |
| Compliance frameworks mapped | ✅ Complete |

---

## 10. Recommendations

1. **Implement account lockout policy** — Lock account after 5 failed login attempts
2. **Enable SSH key-based authentication only** — Disable password authentication for SSH
3. **Deploy firewall rules** — Block SSH access from unknown IPs using allowlisting
4. **Enable Windows Defender alerts** — Forward Defender alerts to Wazuh for better coverage
5. **Implement fail2ban** — Auto-ban IPs after repeated failed attempts

---

## 11. Conclusion

The Wazuh SIEM successfully detected all attack phases in real time with zero false negatives. The Windows endpoint was protected by built-in security controls. No data was exfiltrated and no unauthorized access was achieved. This incident demonstrates the effectiveness of SIEM-based monitoring for detecting brute force and reconnaissance attacks in a hybrid cloud environment.

---

**Report prepared by:** Krup Patel  
**Date:** June 7, 2026  
**Tools used:** Wazuh SIEM 4.7.5, AWS EC2, Kali Linux, Nmap, Hydra, CrackMapExec
