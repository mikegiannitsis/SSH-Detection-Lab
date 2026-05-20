# SOC Detection Lab: Adversary Simulation and TTP Analysis

## Overview
A simulated enterprise environment built to emulate a multi-vector attack combining SSH brute force and credential phishing. Wazuh SIEM was deployed across multiple endpoints to detect, investigate, and document adversary behavior mapped to MITRE ATT&CK, producing structured detection recommendations and incident response findings.

| Tool | Purpose |
|------|---------|
| Wazuh SIEM | Security monitoring, alert generation, and log analysis |
| Kali Linux | Adversary simulation, brute force and phishing |
| Mailhog | Phishing email delivery simulation |
| VMware | Isolated virtualized enterprise network |

---

## Environment

All systems isolated within a private virtualized network for controlled adversary simulation.

| Role | Machine | Purpose |
|------|---------|---------|
| SIEM Manager | Linux Security Server | Wazuh Manager and Dashboard |
| Target Endpoint | Linux Workstation | SSH target and phishing victim |
| Additional Endpoint | Windows Server | Active Directory and endpoint monitoring |
| Adversary Machine | Kali Linux | Attack simulation |

---

## Objectives
- Deploy Wazuh SIEM across a multi endpoint virtualized environment to enable centralized log collection, threat detection, and security monitoring.
- Emulate a realistic two phase adversary attack combining reconnaissance, brute force, and phishing to test detection coverage.
- Map observed adversary behavior to MITRE ATT&CK TTPs and develop custom detection rules to identify and alert on malicious activity.
- Produce structured intelligence findings and detection improvement recommendations based on log analysis and alert investigation.

---

## Adversary Simulation

This lab emulates a realistic two phase attack against a Linux endpoint, simulating the behavior of a threat actor targeting enterprise credentials.

**Phase 1: Reconnaissance and Brute Force Attempt**

The adversary conducted an nmap scan of the target Linux client, confirming port 22 SSH was open, consistent with MITRE ATT&CK T1046 (Network Service Discovery). Initial access was attempted via SSH password guessing, consistent with T1110 (Brute Force), which was unsuccessful due to strong credentials.

<img width="621" height="212" alt="image" src="https://github.com/user-attachments/assets/d5058be9-48d5-4a29-83f5-55dd77dd4e2a" />

**Phase 2: Credential Phishing and Initial Access**

Following the failed brute force, a phishing email was delivered via Mailhog to target user janed@linux-client requesting credential verification, consistent with T1566 (Phishing). The user submitted credentials through the phishing page, which were captured in the creds.log file. The adversary used the harvested credentials to successfully establish SSH access, consistent with T1078 (Valid Accounts).

<img width="607" height="458" alt="image" src="https://github.com/user-attachments/assets/1c2fe1ab-82d5-4293-a120-a26a28f86b96" />

<img width="523" height="95" alt="image" src="https://github.com/user-attachments/assets/599b1443-981c-4d4e-b8e1-e08d59918428" />

<img width="539" height="299" alt="image" src="https://github.com/user-attachments/assets/baf728ea-e96b-4f87-b621-514edd6ab2a8" />

---

## Detection and Investigation in Wazuh

With Wazuh agents deployed across all endpoints, the following adversary activity was captured, correlated, and investigated in the SIEM dashboard and raw log files:

- Identified attacker and victim IP addresses across the virtualized network
- Correlated authentication events — failed brute force attempts followed by successful login using phished credentials
- Triggered custom detection rule alerting on three or more failed SSH login attempts within a defined time window
- Traced the full attack progression from reconnaissance through credential access to initial access confirmation

<img width="610" height="162" alt="image" src="https://github.com/user-attachments/assets/b0ea5aa0-554a-4c16-a8e2-0e57c67005ad" />

<img width="613" height="140" alt="image" src="https://github.com/user-attachments/assets/ac0c4dd7-107f-448a-9f45-65553a0b1ab2" />

<img width="612" height="156" alt="image" src="https://github.com/user-attachments/assets/8242b88c-5d58-4f87-820c-e89a057c8df7" />

---

## Intelligence Summary

| Kill Chain Stage | Adversary Action | MITRE ATT&CK TTP |
|-----------------|------------------|------------------|
| Reconnaissance | nmap scan identifying open SSH port | T1046 |
| Initial Access Attempt | SSH password brute force | T1110 |
| Credential Access | Phishing email delivering fake login page | T1566 |
| Initial Access | SSH login using harvested credentials | T1078 |

---

## Detection Recommendations
- Implement rate limiting and account lockout after repeated failed SSH authentication attempts
- Deploy email security controls to detect and block phishing attempts at the mail server level
- Alert on successful SSH logins preceded by failed attempts from the same source IP
- Monitor for nmap scan signatures and internal port scanning activity
- Expand custom Wazuh rules to detect credential stuffing patterns across multiple endpoints

---

## Skills Demonstrated
`Threat Intelligence` `Adversary Simulation` `MITRE ATT&CK Mapping`
`TTP Analysis` `SIEM Deployment` `Wazuh` `Custom Detection Rules`
`Log Analysis` `Phishing Detection` `Intelligence Reporting`
