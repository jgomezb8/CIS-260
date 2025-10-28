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

<img width="656" height="454" alt="image" src="https://github.com/user-attachments/assets/87f29093-20b4-4bb8-b918-1f8b71c4638c" />


## 4.2 Download and Execute Wazuh Installation Script
curl -sO https://packages.wazuh.com/4.12/wazuh-install.sh && sudo bash ./wazuh-install.sh -a -i

<img width="642" height="464" alt="image" src="https://github.com/user-attachments/assets/ba1ef9ad-97a2-4212-ad01-76c439263c03" />


-a: Installs all components (manager, indexer).
-i: Interactive mode.
The script handles installation and configuration automatically.

After installation finalized, save the User and Password given by Wazuh located at the bottom of the installation output under -- Summary --. You will need this information later.

<img width="642" height="464" alt="image" src="https://github.com/user-attachments/assets/28447923-0a8c-4570-908c-afbaf6c82819" />


## 5. Accessing the Wazuh Dashboard

Check Ubuntu VM IP: ifconfig (or ip addr show).

<img width="642" height="464" alt="image" src="https://github.com/user-attachments/assets/168c107f-4092-4a88-85f7-d70d0d642c9b" />


Open browser on Ubuntu: https://"ubuntu-vm-ip".
Accept self-signed certificate warning.

<img width="642" height="464" alt="image" src="https://github.com/user-attachments/assets/1740fa99-d314-4f60-acff-a30c942b250a" />

Log in with credentials shown at script end.

<img width="642" height="464" alt="image" src="https://github.com/user-attachments/assets/f18be4be-7578-4b7b-95ef-a254e05a5451" />


## 6. Installing the Wazuh Agent (Windows Host)

Download latest MSI from official docs: https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-windows.html MSI on Windows with default settings.

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
