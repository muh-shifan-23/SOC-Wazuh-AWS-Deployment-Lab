# SOC Home Lab: Endpoint Monitoring & Detection using Wazuh SIEM on AWS EC2

## Project Overview

This project demonstrates a cloud-based Security Operations Center (SOC) lab using Wazuh SIEM deployed on AWS EC2 instances running Ubuntu 22.04.

The lab focuses on:

- Endpoint log collection  
- Security event detection  
- Alert analysis  
- Attack simulation and investigation  

### Lab Components

- Wazuh Manager deployed on AWS EC2 (Ubuntu 22.04)
- Wazuh Agent deployed on another AWS EC2 (Ubuntu 22.04)
- Web-based Wazuh Dashboard
- Attack simulation with detection validation

---

## Architecture

EC2 Instance 1 (Wazuh Manager - Ubuntu 22.04)
||
|| TCP/UDP 1514
|| TCP 1515
|| TCP 55000
||
/
EC2 Instance 2 (Wazuh Agent - Ubuntu 22.04)


Logs Flow:
Agent → Manager → Wazuh Dashboard

---

## Tools & Technologies Used

- AWS EC2 (Ubuntu 22.04)
- Wazuh SIEM
- SSH
- Linux
- Security event simulation

---

## Deployment Summary

### Manager Setup

1. Launched Ubuntu 22.04 EC2 instance.
2. Installed Wazuh Manager using official installation script:

curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
sudo bash wazuh-install.sh -a

3. Verified manager services:

sudo systemctl status wazuh-manager
sudo systemctl status wazuh-dashboard


4. Accessed dashboard via:

https://<MANAGER_PUBLIC_IP>:5601

---

### Agent Deployment

1. From Wazuh Dashboard → Agents → Deploy New Agent.
2. Selected “Linux” package.
3. Copied installation command and executed on Agent EC2:

curl -sO https://packages.wazuh.com/4.x/wazuh-agent_4.x.x-1_amd64.deb
sudo WAZUH_MANAGER='<MANAGER_PUBLIC_IP>' dpkg -i wazuh-agent_4.x.x-1_amd64.deb


4. Started and enabled agent:

sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
sudo systemctl status wazuh-agent


5. Confirmed agent appears as **Active** on Dashboard.

---

## Attack Simulations

### SSH Brute Force Simulation

Executed multiple failed login attempts:

for i in {1..50}; do ssh ubuntu@AGENT_IP; done

**Alert Observed:**  
- Rule ID 5710 – SSH authentication failure  
- Severity: High  

**Significance:**  
Indicates potential brute-force attack or unauthorized access attempts.

---

### Privilege Escalation Simulation

Performed:

sudo nano /etc/ssh/sshd_config

sudo systemctl restart ssh

**Alert Observed:**  
- Rule ID 5402 – Successful sudo to root  
- Severity: Medium  

---

## Screenshots (Evidence)


![Dashboard](screenshots/)

---

## Alerts & Analysis

| Rule ID | Description                       | Severity |
|---------|-----------------------------------|----------|
| 5402    | Successful sudo to root           | Medium   |
| 5501    | PAM login session opened          | Medium   |
| 5502    | PAM login session closed          | Medium   |
| 5710    | SSH authentication failure        | High     |

These alerts demonstrate Wazuh’s capability to detect authentication abuse and privilege escalation attempts.

---

## Skills Demonstrated

- AWS Cloud Deployment
- SIEM Installation & Configuration
- Endpoint Monitoring
- Log Analysis
- Linux Administration
- Security Event Investigation
- SOC Operations Fundamentals

---

## Conclusion

This lab demonstrates practical SOC implementation using Wazuh SIEM:

- Manager & Agent deployment in AWS
- Centralized log monitoring
- Real-time alert detection
- Attack simulation with detection validation

This project simulates a foundational Tier-1 SOC Analyst workflow in a cloud environment.

<img width="1740" height="686" alt="image" src="https://github.com/user-attachments/assets/6c1b982f-9763-43da-aab1-fa916f18cf8c" />


