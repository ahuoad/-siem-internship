# Lab Setup Verification

## Description  
Initial configuration of a cybersecurity lab with Wazuh SIEM on Ubuntu and Windows log forwarding, including Sysmon deployment for enhanced monitoring.

## Objective  
Verify proper log collection from Windows to Wazuh SIEM and establish baseline logging capabilities.

## Tools Used  
- **SIEM**: Wazuh 4.7 (Ubuntu 20.04 LTS)  
- **Log Sources**:  
  - Windows Event Log (Security/Sysmon)  
  - Wazuh Agent  
- **Lab Setup**:  
  - VMware VMs: Windows 10 (Target) + Ubuntu (SIEM)  
  - Network: Bridged 


## Config file
- **ossec-agent/ossec.conf**

  The Agent was Configured to Point to Ubuntu by setting the ip address of ubuntu as the server : 
  ![editing_ossec_conf](https://github.com/user-attachments/assets/61c976b3-b823-4dd7-a725-70807078b083)


## Evidence of Successful Setup
- **Wazuh Manager Running**
  
 ![ubuntu_wazuh_status](https://github.com/user-attachments/assets/c47970d5-d383-4fc3-8ddc-5c7baa1274f0)

- **Wazuh Alert**
  
  ![wazuh_alerts_sample](https://github.com/user-attachments/assets/26d15d4e-6b0c-4ff7-98c0-0d6d14e13701)

-**Windows Agent Connected**

 ![windows_agent_installed](https://github.com/user-attachments/assets/7a17bec2-2b27-45a2-8e6b-492602ad6c14)

-**Sysmon server status**

![sysmon_service_started](https://github.com/user-attachments/assets/fcbd14f6-a961-4097-a0b1-5446c0f97249)


