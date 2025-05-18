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
<server>
  <address>192.168.1.9</address>  
  <port>1514</port>
</server>


## Evidence of Successful Setup
- **Wazuh Manager Running**





