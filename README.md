# SOC Detection Lab

## Overview

A simulated SOC monitoring environment built using Wazuh SIEM to detect, investigate, and respond to a multi-vector attack combining SSH brute force and credential phishing against a virtualized enterprise network.

| Tool | Purpose |
|------|---------|
| Wazuh SIEM | Security monitoring, alert generation, and log analysis |
| Kali Linux | Attacker machine for brute force and phishing simulation |
| Mailhog | Mail server used to simulate phishing email delivery |
| VMware | Virtualized private network environment |

---

## Environment

All systems are isolated within a private virtualized network for 
testing purposes only.

| Role | Machine | Purpose |
|------|---------|---------|
| SIEM Manager | Linux Security Server | Wazuh Manager and Dashboard |
| Target Endpoint | Linux Workstation | SSH target and phishing victim |
| Additional Endpoint | Windows Server | Active Directory and endpoint monitoring |
| Attacker Machine | Kali Linux | SSH brute force and phishing attack simulation |

---

## Objectives

- Deploy and configure Wazuh SIEM across a multi-endpoint virtualized 
environment to enable centralized log collection and security monitoring.
- Simulate a multi-vector attack combining SSH brute force and credential 
phishing to test detection capabilities.
- Develop custom detection rules to identify brute force activity and 
trigger structured alert triage and incident response workflows.
- Analyze raw log data to profile attacker behavior and refine detection 
rules to improve visibility and reduce false positives.

---

## Attack Scenario

This lab simulates a realistic two-phase attack against a Linux endpoint:

**Phase 1 — SSH Brute Force Attempt**

The attacker machine performed an nmap scan of the target Linux client, 
confirming port 22 SSH was open. Initial access was attempted via SSH 
password guessing which was unsuccessful due to strong credentials.

<img width="621" height="212" alt="image" src="https://github.com/user-attachments/assets/d5058be9-48d5-4a29-83f5-55dd77dd4e2a" />

**Phase 2 — Credential Phishing via Email**

Following the failed brute force, a phishing email was sent from the 
attacker machine via Mailhog to the target user janed@linux-client, 
requesting credential verification. Once the user submitted their 
credentials through the phishing page, the attacker captured them in 
a creds.log file and used them to successfully SSH into the compromised 
machine.

<img width="607" height="458" alt="image" src="https://github.com/user-attachments/assets/1c2fe1ab-82d5-4293-a120-a26a28f86b96" />

<img width="523" height="95" alt="image" src="https://github.com/user-attachments/assets/599b1443-981c-4d4e-b8e1-e08d59918428" />

<img width="539" height="299" alt="image" src="https://github.com/user-attachments/assets/baf728ea-e96b-4f87-b621-514edd6ab2a8" />

---

## Detection & Investigation in Wazuh

With Wazuh agents deployed across all endpoints, the following activity 
was captured and investigated in the SIEM dashboard and raw log files:

- Identified the target IP of the compromised Linux client
- Identified the source IP of the attacker machine
- Correlated authentication results — failed brute force attempts 
followed by successful login using phished credentials
- Triggered custom detection rule alerting on three or more failed 
SSH login attempts within a defined time window

<img width="610" height="162" alt="image" src="https://github.com/user-attachments/assets/b0ea5aa0-554a-4c16-a8e2-0e57c67005ad" />

<img width="613" height="140" alt="image" src="https://github.com/user-attachments/assets/ac0c4dd7-107f-448a-9f45-65553a0b1ab2" />

<img width="612" height="156" alt="image" src="https://github.com/user-attachments/assets/8242b88c-5d58-4f87-820c-e89a057c8df7" />

---

## Findings & Recommendations

| Finding | Detail |
|---------|--------|
| Initial Recon | nmap scan identified open port 22 on target machine |
| Brute Force Attempt | Multiple failed SSH authentication attempts from attacker IP |
| Credential Phishing | Mailhog used to deliver phishing email and capture credentials |
| Successful Access | SSH login achieved using phished credentials |

**Recommended Detection Improvements:**
- Implement rate limiting and account lockout policies after repeated 
failed SSH authentication attempts
- Deploy email security controls to detect and block phishing attempts 
at the mail server level
- Alert on SSH logins that follow a pattern of prior failed attempts 
from the same source IP
- Monitor for nmap scan signatures and port scanning activity against 
internal endpoints
- Expand custom Wazuh rules to detect credential stuffing patterns 
across multiple endpoints

---

## Skills Demonstrated

`SIEM Deployment` `Wazuh` `Alert Triage` `Incident Response` 
`Custom Detection Rules` `Log Analysis` `Threat Simulation` 
`Phishing Detection` `SSH Brute Force Detection` `Network Monitoring`
