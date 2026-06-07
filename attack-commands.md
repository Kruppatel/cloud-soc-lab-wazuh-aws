# Attack Commands — Cloud SOC Lab

## Phase 1 — Reconnaissance

### Nmap Port Scan
```bash
nmap -sV -sC 192.168.64.10
```
**Purpose:** Discover open ports and services on the target
**Result:** Found ports 135, 139, 445 open — Windows RPC, NetBIOS, SMB

---

## Phase 2 — Brute Force

### SSH Brute Force (Hydra)
```bash
hydra -l administrator -P /usr/share/wordlists/rockyou.txt 192.168.64.10 ssh -t 4 -V
```
**Purpose:** Attempt to crack SSH credentials using common passwords
**Result:** 20 authentication failure alerts generated in Wazuh

### SMB Brute Force (CrackMapExec)
```bash
crackmapexec smb 192.168.64.10 -u administrator -p /usr/share/wordlists/rockyou.txt
```
**Purpose:** Attempt to authenticate to Windows file sharing
**Result:** Windows Firewall blocked NETBIOS connections — target fingerprinted successfully

---

## Phase 3 — Enable SSH on Windows Victim

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
Start-Service sshd
Set-Service -Name sshd -StartupType 'Automatic'
```
**Purpose:** Enable SSH service on Windows 11 to allow brute force testing

---

## Wazuh Agent Installation (Windows PowerShell)

```powershell
Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.7.5-1.msi -OutFile ${env.tmp}\wazuh-agent; msiexec.exe /i ${env.tmp}\wazuh-agent /q WAZUH_MANAGER='3.23.100.47' WAZUH_AGENT_NAME='windows-victim' WAZUH_REGISTRATION_SERVER='3.23.100.47'
NET START WazuhSvc
```
**Purpose:** Install and start Wazuh agent on Windows victim machine

---

## SSH into Wazuh Server (Mac Terminal)

```bash
ssh -i ~/.ssh/soc-lab-key.pem ubuntu@<public-ip>
```

---

## Wazuh Installation Commands (Ubuntu EC2)

```bash
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
curl -sO https://packages.wazuh.com/4.7/config.yml
sudo bash wazuh-install.sh --generate-config-files --ignore-check
sudo bash wazuh-install.sh --wazuh-indexer node-1 --ignore-check
sudo bash wazuh-install.sh --start-cluster --ignore-check
sudo bash wazuh-install.sh --wazuh-server wazuh-1 --ignore-check
sudo bash wazuh-install.sh --wazuh-dashboard dashboard --ignore-check
```
