# CIS-260-Projects

## Overview
Wazuh is a free, open-source security platform providing log analysis, file integrity monitoring, intrusion detection, vulnerability detection, and real-time alerting. This lab builds a basic setup for learning: Wazuh Manager on Ubuntu VM collects and analyzes data; Wazuh Agent on Windows host sends logs and events.

## 2. Lab Architecture

Wazuh Manager: Ubuntu (VirtualBox VM) – Collects, analyzes, and stores agent data.
Wazuh Agent: Windows (host machine) – Forwards logs and system events to the manager.
Network: Use bridged adapter in VirtualBox for same-network access between host and VM.

## 3. Prerequisites

VirtualBox installed.
Ubuntu Server 20.04+ installed in VirtualBox with bridged networking.
Internet access on Ubuntu VM.
Admin access on Windows host.
Optional: Basic Linux/system admin knowledge.

## 4. Installing the Wazuh Manager (Ubuntu)
Run these on the Ubuntu VM terminal.
4.1 Add Wazuh GPG Key
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo gpg --dearmor -o /usr/share/keyrings/wazuh-archive-keyring.gpg
This verifies Wazuh packages.
4.2 Download and Execute Wazuh Installation Script
curl -sO https://packages.wazuh.com/4.12/wazuh-install.sh && sudo bash ./wazuh-install.sh -a -i

-a: Installs all components (manager, indexer).
-i: Interactive mode.
The script handles installation and configuration automatically.

## 5. Accessing the Wazuh Dashboard

Check Ubuntu VM IP: ifconfig (or ip addr show).
Open browser on Ubuntu: https://<ubuntu-vm-ip>.
Accept self-signed certificate warning.
Log in with credentials shown at script end.

## 6. Installing the Wazuh Agent (Windows Host)

Download latest MSI from official docs: https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh_agent_windows.html.
Install MSI on Windows with default settings.

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
