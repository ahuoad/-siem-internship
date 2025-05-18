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
1. **Update System Packages**
sudo apt update && sudo apt upgrade -y
2. **Add Wazuh repository key**
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import && sudo chmod 644 /usr/share/keyrings/wazuh.gpg

3. **Add Wazuh APT repository**
echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | sudo tee -a /etc/apt/sources.list.d/wazuh.list

4. **Install Wazuh Manager**
sudo apt update
sudo apt install -y wazuh-manager

### **Windows Target Machine Configuration**:




## Evidence of Successful Setup
- **Wazuh Manager Running**
  
 ![ubuntu_wazuh_status](https://github.com/user-attachments/assets/c47970d5-d383-4fc3-8ddc-5c7baa1274f0)

- **Wazuh Alert**
  
![wazuh_alerts_sample](https://github.com/user-attachments/assets/26d15d4e-6b0c-4ff7-98c0-0d6d14e13701)

-**Windows Agent Connected**

 ![windows_agent_installed](https://github.com/user-attachments/assets/7a17bec2-2b27-45a2-8e6b-492602ad6c14)

-**Sysmon server status**

![sysmon_service_started](https://github.com/user-attachments/assets/fcbd14f6-a961-4097-a0b1-5446c0f97249)


