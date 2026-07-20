# Dashboard 02 - Windows Endpoint Security Monitoring

## Overview

The **Windows Endpoint Security Monitoring** dashboard provides SOC analysts with centralized visibility into endpoint security detections collected by the Wazuh Manager from Windows endpoints running Sysmon.

Unlike the **Enterprise SOC - Overview** dashboard, which provides a high-level view of the organization's security posture, this dashboard focuses on endpoint-level security detections. It enables analysts to monitor suspicious process execution, PowerShell activity, file creation, registry modifications, authentication events, and other security-relevant behaviors that may indicate malicious activity.

This dashboard serves as the primary workspace for monitoring Windows endpoint detections and supporting security investigations within the Enterprise SOC Lab.

## Objective

- Monitor Windows endpoint security detections in real time.
- Visualize Wazuh alerts generated from Sysmon and Windows Security events.
- Detect suspicious endpoint behavior that may indicate malicious activity.
- Support SOC analysts during security monitoring and incident investigations.
- Provide a centralized dashboard for Windows endpoint security visibility.


# Widget 01 - Process Creation Detections

## Objective

The **Process Creation Detections** widget displays the number of Wazuh detections generated from Sysmon Process Creation (Event ID 1) events over time.

This visualization helps SOC analysts identify suspicious process execution, monitor endpoint activity, detect abnormal process behavior, and quickly recognize potential attacker execution techniques during security investigations.

## Why This Visualization?

Process creation is one of the most valuable sources of endpoint telemetry during incident investigations.

Most attacker actions begin with the execution of a process, such as PowerShell, Command Prompt, LOLBins, or custom malware. Monitoring Sysmon Event ID 1 detections enables SOC analysts to quickly identify suspicious execution activity and begin investigating the affected endpoint.