# Azure-Cloud-SOC

# Building a SOC + Honeynet in Azure (Live Traffic)
![azure honeynet](https://github.com/imadeda/Azure-Cloud-Soc/assets/152673708/b6650c33-9b03-41aa-8e4e-c0c495e5ed02)


## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

![BEFORE HARDENING](https://github.com/imadeda/Azure-Cloud-Soc/assets/152673708/a14d28a3-4283-496f-8f82-77854027ae67)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

![AFTER HARDNEING](https://github.com/imadeda/Azure-Cloud-Soc/assets/152673708/2a867f7d-9299-49ed-b946-d003a74097aa)


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls

<img width="1249" alt="(before)-nsg-malicious-allowed-in" src="https://github.com/imadeda/Azure-Cloud-Soc/assets/152673708/909c25de-76ee-46da-94f5-2ecbab4c5347"> <br>
<img width="1180" alt="(before)-linux-ssh-auth-fail" src="https://github.com/imadeda/Azure-Cloud-Soc/assets/152673708/d920a565-872d-4a1f-9b3b-4316167829b5"> <br>
<img width="1116" alt="(before)-windows-rdp-auth-fail" src="https://github.com/imadeda/Azure-Cloud-Soc/assets/152673708/e44a50d6-8015-42d2-8a7e-44126a015317">




## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:<br>
Start Time 2023-11-27 18:22<br>
Stop Time 2023-11-28 18:22<br>

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 2138
| Syslog                   | 4284
| SecurityAlert            | 1
| SecurityIncident         | 109
| AzureNetworkAnalytics_CL | 1968

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-11-29 20:52<br>
Stop Time	2023-11-30 20:52</br>

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 120
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
