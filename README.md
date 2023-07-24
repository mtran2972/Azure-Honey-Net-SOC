# Building a SOC + Honeynet in Azure (Live Traffic)
![Untitled](https://github.com/mtran2972/Azure-Honey-Net-SOC/assets/137732132/58eb2720-4b5e-4c2e-b9c7-1dfdb6a72b51)

## Introduction

In this project, I constructed a miniature honeynet using Azure and ingested log sources from various resources into a Log Analytics workspace. Microsoft Sentinel played a crucial role in creating attack maps, activating alerts, and generating incidents. Initially, I assessed security metrics in the vulnerable environment for a 24-hour period. Subsequently, I implemented security controls to strengthen the environment and then reevaluated the security metrics for an additional 24 hours. The metrics presented in this study include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Technologies, Azure components, and Regulations Employed: 
- Azure Virtual Network (VNet)
= Azure Network Security Group (NSG)
- Virtual Machines (2x Windows, 1x Linux)
- Log Analytics Workspace with Kusto Query Language (KQL) Queries
- Azure Key Vault for Secure Secrets Management
- Azure Storage Account for Data Storage
- Microsoft Sentinel for Security Information and Event Management (SIEM)
- Microsoft Defender for Cloud to Protect Cloud Resources
- Windows Remote Desktop for Remote Access
- Command Line Interface (CLI) for System Management
- PowerShell for Automation and Configuration Management
- NIST SP 800-53 Revision 5 for Security Controls
- NIST SP 800-61 Revision 2 for Incident Handling Guidance

## Architecture Before Hardening / Security Controls
![prior](https://github.com/mtran2972/Azure-Honey-Net-SOC/assets/137732132/c7c2a14b-9d22-43f1-b3ec-1a15b3eea4a8)

In the "BEFORE" stage of the project, a virtual environment was deliberately deployed and exposed to the public to attract malicious actors and study their attack patterns. To achieve this, a Windows virtual machine hosting a SQL database and a Linux server were set up with network security groups (NSGs) configured to "Allow All." Additionally, a storage account and key vault were deployed with public endpoints visible on the open internet to further entice potential attackers. During this stage, Microsoft Sentinel monitored the unsecured environment using logs aggregated by the Log Analytics workspace.

## Architecture After Hardening / Security Controls
![after](https://github.com/mtran2972/Azure-Honey-Net-SOC/assets/137732132/ab1649b0-0508-4a34-8c02-f59df7523cc2)

In the "AFTER" stage of the project, security measures were implemented to meet NIST SP 800-53 Rev4 SC-7(3) Access Points. The hardening tactics included:

- Network Security Groups (NSGs): NSGs were hardened by blocking all inbound and outbound traffic with the exception of designated public IP addresses that required access to the virtual machines. This ensured that only authorized traffic from a trusted source was allowed to access the virtual machines.
  
- Built-in Firewalls: Azure's built-in firewalls were configured on the virtual machines to restrict unauthorized access and protect the resources from malicious connections. This step involved fine-tuning the firewall rules based on the service and responsibilities of each VM which mitigated the attack surface bad actors had access to.
  
- Private Endpoints: To enhance the security of Azure Key Vault and Storage Containers, Public Endpoints were replaced with Private Endpoints. This ensured that access to these sensitive resources was limited to the virtual network and not the public internet.

## Attack Maps Before Hardening / Security Controls
![mssql auth fail](https://github.com/mtran2972/Azure-Honey-Net-SOC/assets/137732132/7b902b4c-7876-4026-aaaa-ccacb9b650c2)
![NSG Malicious allowed in](https://github.com/mtran2972/Azure-Honey-Net-SOC/assets/137732132/6e86ff36-b700-48d7-bcfe-4bd0bc0b4b2f)
![syslog ssh auth fail](https://github.com/mtran2972/Azure-Honey-Net-SOC/assets/137732132/30c41899-2b08-4c10-87c7-cfa97942a839)
![window rdp auth fail](https://github.com/mtran2972/Azure-Honey-Net-SOC/assets/137732132/51a1b994-d3ec-4f62-b6d2-d54041504a8c)


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 7/21/2023 12:40:28
Stop Time 7/22/2023 12:40:28

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 83905
| Syslog                   | 39
| SecurityAlert            | 3
| SecurityIncident         | 2302
| AzureNetworkAnalytics_CL | 788

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 7/22/2023 17:06:23
Stop Time	7/23/2023 17:06:23

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8372
| Syslog                   | 3
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion


In this project, a compact yet highly effective honeynet was created on Microsoft Azure, with log sources seamlessly integrated into a Log Analytics workspace. Leveraging Microsoft Sentinel, the system was configured to promptly trigger alerts and generate incidents based on the ingested logs. The project involved evaluating metrics in an unsecured environment before applying security controls, followed by measuring the impact after implementing these robust security measures.

The results were nothing short of impressive. After the implementation of these enhanced security controls, there was a substantial 90% reduction in Windows Security Events, a notable 92% reduction in Linux Events, and a complete eradication of security alerts, incidents, and malicious inbound network traffic - achieving an astounding 100% reduction. These findings serve as compelling evidence of the effectiveness of these security measures in fortifying the environment and providing formidable protection against potential cyber threats.
