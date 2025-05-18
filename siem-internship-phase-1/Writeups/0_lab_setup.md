# Lab Setup Verification

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
### **Ubuntu SIEM Server Configuration**:


### **Windows Target Machine Configuration**:


1. Update System Packages

```bash
sudo apt update && sudo apt upgrade -y
```

2. Install Wazuh Manager

```bash
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import
```

```bash
sudo chmod 644 /usr/share/keyrings/wazuh.gpg
```

```bash
echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | sudo tee -a /etc/apt/sources.list.d/wazuh.list
```

```bash
sudo apt update
```

```bash
sudo apt install -y wazuh-manager
```

3. Verify Wazuh Manager Installation

```bash
sudo systemctl status wazuh-manager
```

Expected output: `Active: active (running)`

---

Windows Target Machine Setup

1. Install Wazuh Agent

- Download from: [Wazuh Agent MSI](https://packages.wazuh.com/4.x/windows/wazuh-agent-4.7.2-1.msi)
- Run the installer with default settings

2. Configure Wazuh Agent

Edit `C:\Program Files (x86)\ossec-agent\ossec.conf`:

```xml
<server>
  <address>YOUR_UBUNTU_IP</address>
  <port>1514</port>
</server>
```

3. Restart Agent Service

```powershell
Restart-Service -Name WazuhSvc
```

```powershell
Get-Service -Name WazuhSvc | Select-Object Status
```

Expected output: `Status: Running`

---

Verification & Enhanced Logging

1. Check Log Forwarding from Agent

```bash
sudo tail -f /var/ossec/logs/alerts/alerts.log
```

Look for new events from the Windows agent

2. Install Sysmon on Windows

- Download from: [Sysmon Download](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
- Extract to `C:\Sysmon`

3. Configure and Run Sysmon

```powershell
cd C:\Sysmon
```

```powershell
curl -o sysmonconfig.xml https://raw.githubusercontent.com/SwiftOnSecurity/sysmon-config/master/sysmonconfig-export.xml
```

```powershell
.\Sysmon.exe -accepteula -i sysmonconfig.xml
```

4. Verify Sysmon Logging

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 1
```

Should show a Sysmon startup or configuration event


## Evidence of Successful Setup
- **Wazuh Manager Running**
  
 ![ubuntu_wazuh_status](https://github.com/user-attachments/assets/c47970d5-d383-4fc3-8ddc-5c7baa1274f0)

- **Wazuh Alert**
  
![wazuh_alerts_sample](https://github.com/user-attachments/assets/26d15d4e-6b0c-4ff7-98c0-0d6d14e13701)

-**Windows Agent Connected**

 ![windows_agent_installed](https://github.com/user-attachments/assets/7a17bec2-2b27-45a2-8e6b-492602ad6c14)

-**Sysmon server status**

![sysmon_service_started](https://github.com/user-attachments/assets/fcbd14f6-a961-4097-a0b1-5446c0f97249)


