# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I used Azure to build and simulate a LIVE honeynet and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build a series of attack maps, trigger alerts, and create incidents. This was done by creating Virtual Machines and exposing them to an insecure environment for 24 hours, then measuring some security metrics. Afterward, some NIST 800-53 security controls were applied to harden and secure the environment, and then I measured the security metrics for another 24 hours. The results are shown below. 

The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 Windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, and exposed to the internet. The Virtual Machines had both of their Network Security Groups and built-in firewalls wide open, and all other resources were deployed with public endpoints visible to the Internet; Hence, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![(BEFORE)-windows-rdp-auth-fail](https://github.com/MohammedAl13/Cloud-SOC/assets/154714127/c284a2b9-f8ef-400d-9260-4d6a6ff97bcc)
<br>
![(BEFORE)-nsg-malicious-allowed-in](https://github.com/MohammedAl13/Cloud-SOC/assets/154714127/ac735774-d3f0-4a1e-a221-4b834872414b)
<br>
![(BEFORE)-mssql-auth-fail](https://github.com/MohammedAl13/Cloud-SOC/assets/154714127/be87e5e8-b651-4190-9660-78c4f037f9de)
![(BEFORE)-linux-ssh-auth-fail1](https://github.com/MohammedAl13/Cloud-SOC/assets/154714127/66902508-09dc-4231-9995-26e80c4f34d0)


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:


Start Time: 2023-12-18 08:20:09


Stop Time:  2023-12-19 08:20:09

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 50656
| Syslog                   | 943
| SecurityAlert            | 5
| SecurityIncident         | 26
| AzureNetworkAnalytics_CL | 129482

## Attack Maps After Hardening / Security Controls

```All map queries returned no results due to no instances of malicious activity for the 24-hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics I measured in my environment for another 24 hours after NIST 800-53 security controls were applied:


Start Time: 2023-12-21 01:57:37


Stop Time:  2023-12-22 01:57:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 12541
| Syslog                   | 5
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0


## Percentage of Security Events 


The following table compares the security events from before hardening the environment vs afterward and shows to what percent were security events reduced:


| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | -75.24%
| Syslog                   | -99.47%
| SecurityAlert            | -100%
| SecurityIncident         | -100%
| AzureNetworkAnalytics_CL | -100%



## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. The firewalls and NSGs for the VMS were disabled, leaving them exposed to the public internet. Microsoft Sentinel was then employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. NIST 800-53 security controls were implemented to harden the environment. It is noteworthy that the number of security events and incidents were drastically reduced after the NIST 800-53 security controls were applied, demonstrating their effectiveness.
