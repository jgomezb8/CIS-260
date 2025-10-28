# CIS-260-Projects

## Overview
Wazuh is a free, open-source security platform providing log analysis, file integrity monitoring, intrusion detection, vulnerability detection, and real-time alerting. This lab builds a basic setup for learning: Wazuh Manager on Ubuntu VM collects and analyzes data; Wazuh Agent on Windows host sends logs and events.

## 2. Lab Architecture

Wazuh Manager: Ubuntu (VirtualBox VM) – Collects, analyzes, and stores agent data.
Wazuh Agent: Windows (host machine) – Forwards logs and system events to the manager.


## 3. Prerequisites

VirtualBox installed.
Ubuntu Server installed in VirtualBox with bridged networking.
Internet access on Ubuntu VM.
Admin access on Windows host.


## 4. Installing the Wazuh Manager (Ubuntu)
Run these commands on the Ubuntu VM terminal.
## 4.1 Add Wazuh GPG Key
Run on terminal: curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo gpg --dearmor -o /usr/share/keyrings/wazuh-archive-keyring.gpg
This verifies Wazuh packages.

<img width="598" height="382" alt="image" src="https://github.com/user-attachments/assets/2fa39960-3775-4938-82fc-b9a77ca8e3fa" />

## 4.2 Download and Execute Wazuh Installation Script
curl -sO https://packages.wazuh.com/4.12/wazuh-install.sh && sudo bash ./wazuh-install.sh -a -i

<img width="608" height="382" alt="image" src="https://github.com/user-attachments/assets/4c9edfc6-0467-4d50-9ab9-0e37ba0c7364" />


-a: Installs all components (manager, indexer).
-i: Interactive mode.
The script handles installation and configuration automatically.

## 5. Accessing the Wazuh Dashboard

Check Ubuntu VM IP: ifconfig (or ip addr show).

<img width="608" height="382" alt="image" src="https://github.com/user-attachments/assets/8d919344-d7a9-496e-b20b-6961b304d9a7" />

Open browser on Ubuntu: https://<ubuntu-vm-ip>.
Accept self-signed certificate warning.
Log in with credentials shown at script end.

## 6. Installing the Wazuh Agent (Windows Host)

Download latest MSI from official docs: https://documentation.wazuh.com/current/installation-guide/wazuh-agent/index.html MSI on Windows with default settings.

## 7. Registering the Agent with the Manager
## 7.1 Generate Agent Key on Ubuntu Manager
sudo /var/ossec/bin/manage_agents

Select A to add agent.
Name it (e.g., WindowsHost).
Leave IP blank (unless static needed).
Select E to extract key.
Copy the key.

## 7.2 Apply Key in the Windows Agent

Open Wazuh Agent Manager GUI from Start Menu.
Paste key into field.
Save and apply.
Add manager's IP (Ubuntu VM IP).
Restart agent service.
Agent should appear in Wazuh dashboard.


## 8. Verifying Setup

Open Wazuh dashboard in browser.
Go to Agents: Confirm Windows agent listed as Active.
Go to Integrity Monitoring section.
In monitored folder (e.g., C:\Users<your-username>\Test), create/modify/delete files.
Check dashboard for alerts.
