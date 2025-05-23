# Lab Setup 

## Introduction
In cybersecurity operations, centralizing and analyzing logs from multiple systems is critical for detecting threats, investigating incidents, and ensuring compliance. This lab sets up a Security Information and Event Management (SIEM) environment using Wazuh, an open-source SIEM platform, to collect and analyze security events from both Linux and Windows hosts. On Windows systems, Sysmon is deployed to enhance native logging by providing detailed information about process creation, network connections, and system changes. Together, Wazuh and Sysmon provide a powerful, real-time monitoring and alerting system for threat detection and visibility across the network.


## Description  
Initial configuration of a cybersecurity lab with Wazuh SIEM on Ubuntu and Windows log forwarding, including Sysmon deployment for enhanced monitoring.

## Objective  
Verify proper log collection from Windows to Wazuh SIEM and establish baseline logging capabilities.

## Tools Used
| Component       | Tool/Version         | Purpose                        |
|-----------------|----------------------|--------------------------------|
| SIEM            | Wazuh 4.7            | Log collection & analysis      |
| Target OS       | Windows 10/11        | Attack simulation              |
| Logging         | Sysmon + Wazuh Agent | Process/network monitoring     |
| Virtualization  | VMware               | Host VMs                       |


## Setup Steps

### Ubuntu SIEM Server Configuration

1. Update System Packages

```bash
# Update package index and upgrade all packages
sudo apt update && sudo apt upgrade -y
```

2. Install Wazuh Manager

```bash
# Download and import Wazuh GPG key
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import
```

```bash
# Set correct permissions on the keyring file
sudo chmod 644 /usr/share/keyrings/wazuh.gpg
```

```bash
# Add Wazuh repository to sources list
echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | sudo tee -a /etc/apt/sources.list.d/wazuh.list
```

```bash
# Update package list with Wazuh repo
sudo apt update
```

```bash
# Install the Wazuh manager
sudo apt install -y wazuh-manager
```

3. Verify Wazuh Manager Installation

```bash
# Check if the Wazuh manager service is active
sudo systemctl status wazuh-manager
```


![status](https://github.com/user-attachments/assets/b6cfbe11-7403-4597-8ffe-7cee07c8f5ea)

 
---

### Windows Target Machine Setup

1. Install Wazuh Agent

- Download from: [Wazuh Agent MSI](https://packages.wazuh.com/4.x/windows/wazuh-agent-4.7.2-1.msi)
- Run the installer with default settings

2. Configure Wazuh Agent

Edit `C:\Program Files (x86)\ossec-agent\ossec.conf`:

```xml
Configure agent to connect to Wazuh manager 
<server>
  <address>UBUNTU_IP</address>
  <port>1514</port>
</server>
```

![editing_ossec_conf](https://github.com/user-attachments/assets/2dc218d4-8d26-4915-a162-e1923f85dd36)


3. Restart Agent Service

```powershell
# Restart the Wazuh agent service
Restart-Service -Name WazuhSvc
```

```powershell
# Check the current status of the Wazuh service
Get-Service -Name WazuhSvc | Select-Object Status
```

 ![windows_agent_installed](https://github.com/user-attachments/assets/7a17bec2-2b27-45a2-8e6b-492602ad6c14)
 
---

Verification & Enhanced Logging

1. Check Log Forwarding from Agent

```bash
# Tail the Wazuh alert log to see incoming agent data
sudo tail -f /var/ossec/logs/alerts/alerts.log
```
Look for new events from the Windows agent

![wazuh_alerts_sample](https://github.com/user-attachments/assets/26d15d4e-6b0c-4ff7-98c0-0d6d14e13701)


2. Install Sysmon on Windows

- Download from: [Sysmon Download](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
- Extract to `C:\Sysmon`

3. Configure and Run Sysmon

```powershell
# Navigate to Sysmon directory
cd C:\Sysmon
```

```powershell
# Download default Sysmon config from SwiftOnSecurity
curl -o sysmonconfig.xml https://raw.githubusercontent.com/SwiftOnSecurity/sysmon-config/master/sysmonconfig-export.xml
```

```powershell
# Install Sysmon with the config file
.\Sysmon.exe -accepteula -i sysmonconfig.xml
```

4. Verify Sysmon Logging

```powershell
# View the latest Sysmon event in Event Viewer
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 1
```
![sysmon_service_started](https://github.com/user-attachments/assets/fcbd14f6-a961-4097-a0b1-5446c0f97249)


  







