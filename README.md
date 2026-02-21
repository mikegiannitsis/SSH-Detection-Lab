# SSH-Detection-Lab

## Overview:
This project demonstrates a SOC monitoring lab using Wazuh to detect and investigate attacks in a contained virtualized environment. I have Wazuh agents installed on Linux and Windows machines, along with a Windows Server. The attacker machine is Kali Linux used to generate SSH login attempts before and after a phishing email was sent to the Linux machine user.

The goal of this lab is to show endpoint visibility and investigation into a rogue SSH log in.

### Objectives:
- Deploy a security monitoring platform
- Detect and investigate SSH brute force activity
- Demonstrate SOC alerting and recognition

### Environment: All systems are isolated and done in a private virtualized network for testing purposes only.

| Role                                         | Purpose        |
|-----------------------------------------------|----------------------------|
| Linux Security Server          | Wazuh Manager and Dashboard |
| Linux Workstation Endpoint | SSH Targert Machine |
| Windows Server Endpoint         | Active Directory Server and Additional Endpoint |
| Kali Linux Attacker      | Attacker Machine for SSH traffic |

### Scenario:
The Attacker VM was used to brute force the Linux client and gain access to the machine. After a nmap scan of the target IP, I see that port 22 SSH is open. Initial access was first attempted by guessing the password which was unsuccessful.

<img width="621" height="212" alt="image" src="https://github.com/user-attachments/assets/d5058be9-48d5-4a29-83f5-55dd77dd4e2a" />

I had a mail server set up with Mailhog which from the attacker machine I sent a “verify password” email to janed@linux-client. Once the credentials were entered and “verified”, I was able to gather those in the creds.log file which allowed me to gain initial access and ssh into the janed client.

<img width="607" height="458" alt="image" src="https://github.com/user-attachments/assets/1c2fe1ab-82d5-4293-a120-a26a28f86b96" />

<img width="523" height="95" alt="image" src="https://github.com/user-attachments/assets/599b1443-981c-4d4e-b8e1-e08d59918428" />

<img width="539" height="299" alt="image" src="https://github.com/user-attachments/assets/baf728ea-e96b-4f87-b621-514edd6ab2a8" />

### Detection in Wazuh:
I am able to review the alerts and raw log file for suspicious SSH log in attempts while identifying the source IP of the attacker and which machine is compromised.

- See the target IP to the Linux Client
- See the Source IP of the attacker machine
- The authentication results: Failed attempted from the brute force | Successful attempts from the phished credentials
- Custom rule for 3 or more failed login attempts

<img width="610" height="162" alt="image" src="https://github.com/user-attachments/assets/b0ea5aa0-554a-4c16-a8e2-0e57c67005ad" />

<img width="613" height="140" alt="image" src="https://github.com/user-attachments/assets/ac0c4dd7-107f-448a-9f45-65553a0b1ab2" />

<img width="612" height="156" alt="image" src="https://github.com/user-attachments/assets/8242b88c-5d58-4f87-820c-e89a057c8df7" />

### Limitations to the project:
- Small example of an “Enterprise” network.
- No firewall installed. Can expand by including a firewall
