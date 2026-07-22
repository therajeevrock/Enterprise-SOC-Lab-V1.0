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

## Dashboard Information

| Property | Value |
|----------|-------|
| Dashboard Name | Enterprise SOC - Windows Endpoint Security Monitoring |
| Platform | Wazuh Dashboard |
| Visualization Engine | OpenSearch Dashboards |
| Data Source | wazuh-alerts-* |
| Time Filter | Last 24 Hours |

# Widget 01 - Process Creation Detections

## Objective

The **Process Creation Detections** widget displays the number of Wazuh detections generated from Sysmon Process Creation (Event ID 1) events over time.

This visualization helps SOC analysts identify suspicious process execution, monitor endpoint activity, detect abnormal process behavior, and quickly recognize potential attacker execution techniques during security investigations.

## Why This Visualization?

Process creation is one of the most valuable sources of endpoint telemetry during incident investigations.

Most attacker actions begin with the execution of a process, such as PowerShell, Command Prompt, LOLBins, or custom malware. Monitoring Sysmon Event ID 1 detections enables SOC analysts to quickly identify suspicious execution activity and begin investigating the affected endpoint.

---

## Create a New Dashboard

Open the Wazuh Dashboard and navigate to:

```text
Dashboard
    └── Create dashboard
```

Click **Create new dashboard**, then enter the following dashboard name:

**Title**

```text
Enterprise SOC - Windows Endpoint Security Monitoring
```

**Description**

```text
Provides centralized visibility into Windows endpoint security detections collected by Wazuh and Sysmon. This dashboard helps SOC analysts monitor suspicious process execution, PowerShell activity, file and registry modifications, authentication events, and other endpoint-related detections to support security monitoring and incident investigations.
```

Click **Create** to create an empty dashboard. The visualizations created in the following steps will be added to this dashboard.

### Screenshot

**Image #01**

```text
dashboards/screenshots/dashboard-02/create-dashboard.png
```

## Step 1 - Create a New Visualization

Open the Wazuh Dashboard and navigate to:

```text
Explore
    └── Visualize
```

Click **Create visualization** to begin creating a new endpoint security visualization.

### Screenshot

**Image #01**

```text
dashboards/screenshots/dashboard-02/01-open-visualize.png
```

## Step 2 - Select the Visualization Type

Select the following visualization type:

```text
Line
```

A **Line** visualization is ideal for displaying security detections over time. It enables SOC analysts to identify spikes in suspicious activity, monitor detection trends, and correlate endpoint events during investigations.

### Screenshot

**Image #02**

```text
dashboards/screenshots/dashboard-02/02-select-line-visualization.png
```

## Step 3 - Select the Data Source

Choose the following data source:

```text
wazuh-alerts-*
```

The **wazuh-alerts-\*** index stores security alerts generated after Wazuh detection rules have matched incoming events. Since this dashboard is designed for endpoint security monitoring rather than raw telemetry analysis, the alerts index provides a focused view of actionable security detections.

### Why use `wazuh-alerts-*` instead of `wazuh-archives-*`?

The `wazuh-alerts-*` index contains only events that triggered Wazuh detection rules, making it ideal for SOC analysts who need to monitor suspicious endpoint activity and prioritize investigations.

In contrast, the `wazuh-archives-*` index stores all collected events, including normal operating system activity. While useful for threat hunting and deep forensic investigations, it introduces significant noise for day-to-day SOC monitoring.

### Screenshot

**Image #03**

```text
dashboards/screenshots/dashboard-02/03-select-alerts-data-source.png
```

## Step 4 - Add a Filter

To display only Sysmon Process Creation detections, add the following filter:

**Field**

```text
rule.groups
```

**Operator**

```text
is
```

**Value**

```text
sysmon_eid1_detections
```

This filter limits the visualization to Wazuh detections generated from **Sysmon Event ID 1 (Process Create)** events. As a result, the graph displays only process creation detections instead of all endpoint security alerts.

### Screenshot

**Image #04**

```text
dashboards/screenshots/dashboard-02/04-add-filter.png
```

## Step 5 - Configure the Y-Axis

Configure the metric as follows:

**Aggregation**

```text
Count
```

The **Count** aggregation displays the total number of process creation detections that match the selected filter during the chosen time range.

### Screenshot

**Image #05**

```text
dashboards/screenshots/dashboard-02/05-configure-y-axis.png
```

## Step 6 - Configure the X-Axis

Configure the horizontal axis using the following settings:

**Aggregation**

```text
Date Histogram
```

**Field**

```text
timestamp
```

**Minimum Interval**

```text
Auto
```

**Drop partial buckets**

```text
Off
```

The **Date Histogram** groups detections over time, allowing SOC analysts to observe trends, identify activity spikes, and correlate suspicious endpoint events during an investigation.

### Screenshot

**Image #06**

```text
dashboards/screenshots/dashboard-02/06-configure-x-axis.png
```

## Step 7 - Save the Visualization

After verifying the visualization configuration, click the **Save** button.

Enter the following details:

**Title**

```text
SOC - Process Creation Detections
```
**Description**

```text
Displays the number of Sysmon Event ID 1 detections generated by Wazuh over time. This visualization helps SOC analysts identify suspicious process execution activity and investigate endpoint behavior.
```

enable `add to Dashboards after saving`

Click **Save and return** to store the visualization in the Wazuh Dashboard.

Using descriptive and consistent visualization names makes dashboard management easier as the number of visualizations grows within the SOC environment.

### Screenshot

**Image #07**

```text
dashboards/screenshots/dashboard-02/07-save-visualization.png
```

## Step 8 - Add the Visualization to the Dashboard

Navigate to:

```text
Dashboard
    └── Enterprise SOC - Windows Endpoint Security Monitoring
```

Click **Edit**, then select **Add** and choose the **SOC - Process Creation Detections** visualization created in the previous step.

Resize and position the widget according to your preferred dashboard layout.

Click **Save** to update the dashboard.

### Screenshot

**Image #08**

```text
dashboards/screenshots/dashboard-02/08-add-visualization-to-dashboard.png
```

## Final Result

After completing the above steps, the dashboard displays a time-series visualization of Sysmon Process Creation detections. This enables SOC analysts to monitor endpoint process execution trends, identify unusual spikes, and quickly begin investigations into suspicious activity.

### Screenshot

**Image #09**

```text
dashboards/screenshots/dashboard-02/09-process-creation-widget.png
```
---

# Widget 02 - PowerShell Detections

## Objective

The **PowerShell Detections** widget displays the number of Wazuh detections related to PowerShell activity over time.

This visualization helps SOC analysts identify suspicious PowerShell execution, monitor script-based attacks, detect malicious command execution, and investigate PowerShell activity across monitored Windows endpoints.

## Why This Visualization?

PowerShell is one of the most commonly abused tools by attackers because it is built into Windows and provides powerful administrative capabilities.

Monitoring PowerShell detections enables SOC analysts to quickly identify suspicious scripts, encoded commands, and other potentially malicious PowerShell activity that may indicate an ongoing attack.

---

## Visualization Configuration

The visualization configuration is identical to **Widget 01 - Process Creation Detections**.

Use the same configuration for:

- Visualization Type
- Data Source
- Y-Axis
- X-Axis
- Save Options

Only the following settings need to be changed.

---

## Step 1 - Update the Filter

Configure the following filter:

**Field**

```text
data.win.eventdata.originalFileName
```

**Operator**

```text
is
```

**Value**

```text
PowerShell.EXE
```

Replace **`<PowerShell Rule Group>`** with the appropriate `data.win.eventdata.originalFileName` value used by your PowerShell detection rule.

### Screenshot

**Image #10**

```text
dashboards/screenshots/dashboard-02/10-powershell-filter.png
```

---

## Step 2 - Save the Visualization

Save the visualization using the following details.

**Title**

```text
SOC - PowerShell Detections
```

**Description**

```text
Displays PowerShell-related detections generated by Wazuh over time. This visualization helps SOC analysts identify suspicious PowerShell execution and investigate script-based attacks.
```

Enable:

```text
Add to Dashboards after saving
```

### Screenshot

**Image #11**

```text
dashboards/screenshots/dashboard-02/11-save-powershell-visualization.png
```

---

## Final Result

After saving the visualization, it will be added to the **Enterprise SOC - Windows Endpoint Security Monitoring** dashboard.

The widget provides a timeline of PowerShell detections, enabling SOC analysts to quickly identify suspicious PowerShell activity across monitored Windows endpoints.

### Screenshot

**Image #12**

```text
dashboards/screenshots/dashboard-02/12-powershell-widget.png
```

# Widget 03 - File Creation Detections

## Objective

The **File Creation Detections** widget displays the number of Wazuh detections generated from Sysmon File Create (Event ID 11) events over time.

This visualization helps SOC analysts monitor newly created files, detect suspicious file drops, identify malware payload deployment, and investigate unauthorized file creation activity across monitored Windows endpoints.

## Why This Visualization?

Attackers frequently create or drop files on compromised systems during various stages of an attack, including malware deployment, persistence, lateral movement, and command-and-control operations.

Monitoring Sysmon File Create detections enables SOC analysts to quickly identify suspicious file creation activity, investigate unexpected executable or script files, and detect potential malware before it executes.

---

## Visualization Configuration

The visualization configuration is identical to **Widget 01 - Process Creation Detections**.

Use the same configuration for:

- Visualization Type
- Data Source
- Y-Axis
- X-Axis
- Save Options

Only the following settings need to be changed.

---

## Step 1 - Update the Filter

Configure the following filter:

**Field**

```text
rule.groups
```

**Operator**

```text
is
```

**Value**

```text
sysmon_eid11_detections
```

This filter displays only Wazuh detections generated from **Sysmon Event ID 11 (File Create)** events.

### Screenshot

**Image #13**

```text
dashboards/screenshots/dashboard-02/13-file-creation-filter.png
```

---

## Step 2 - Save the Visualization

Save the visualization using the following details.

**Title**

```text
SOC - File Creation Detections
```

**Description**

```text
Displays the number of Sysmon Event ID 11 detections generated by Wazuh over time. This visualization helps SOC analysts identify suspicious file creation activity and investigate potential malware deployment.
```

Enable:

```text
Add to Dashboards after saving
```

### Screenshot

**Image #14**

```text
dashboards/screenshots/dashboard-02/14-save-file-creation-visualization.png
```

---

## Final Result

After saving the visualization, it will be added to the **Enterprise SOC - Windows Endpoint Security Monitoring** dashboard.

The widget provides a timeline of Sysmon File Create detections, enabling SOC analysts to identify unusual file creation activity, detect potential malware drops, and investigate suspicious endpoint behavior.

### Screenshot

**Image #15**

```text
dashboards/screenshots/dashboard-02/15-file-creation-widget.png
```

# Widget 04 - Registry Modification Detections

## Objective

The **Registry Modification Detections** widget displays the number of Wazuh detections generated from Sysmon Registry Event (Event ID 13) events over time.

This visualization helps SOC analysts monitor registry modifications, detect persistence techniques, identify unauthorized registry changes, and investigate suspicious activity across monitored Windows endpoints.

## Why This Visualization?

The Windows Registry is frequently targeted by attackers to establish persistence, modify system behavior, disable security controls, or execute malicious code during system startup.

Monitoring Sysmon Registry Modification detections enables SOC analysts to quickly identify suspicious registry changes, investigate persistence mechanisms, and detect potential attacker activity before it impacts the endpoint.

Registry modifications under locations such as `HKLM\System\CurrentControlSet\Services` are particularly important because they may indicate the creation or modification of Windows services used to achieve persistence.

---

## Visualization Configuration

The visualization configuration is identical to **Widget 01 - Process Creation Detections**.

Use the same configuration for:

- Visualization Type
- Data Source
- Y-Axis
- X-Axis
- Save Options

Only the following settings need to be changed.

---

## Step 1 - Update the Filter

Configure the following filter:

**Field**

```text
rule.groups
```

**Operator**

```text
is
```

**Value**

```text
sysmon_eid13_detections
```

This filter displays only Wazuh detections generated from **Sysmon Event ID 13 (Registry Value Set)** events.

### Screenshot

**Image #16**

```text
dashboards/screenshots/dashboard-02/16-registry-filter.png
```

---

## Step 2 - Save the Visualization

Save the visualization using the following details.

**Title**

```text
SOC - Registry Modification Detections
```

**Description**

```text
Displays the number of Sysmon Event ID 13 detections generated by Wazuh over time. This visualization helps SOC analysts identify suspicious registry modifications and investigate persistence-related activity.
```

Enable:

```text
Add to Dashboards after saving
```

### Screenshot

**Image #17**

```text
dashboards/screenshots/dashboard-02/17-save-registry-visualization.png
```

---

## Final Result

After saving the visualization, it will be added to the **Enterprise SOC - Windows Endpoint Security Monitoring** dashboard.

The widget provides a timeline of Sysmon Registry Modification detections, enabling SOC analysts to identify suspicious registry changes, investigate persistence techniques, and detect unauthorized modifications on monitored Windows endpoints.

### Screenshot

**Image #18**

```text
dashboards/screenshots/dashboard-02/18-registry-widget.png
```

# Widget 05 - Failed Authentication Detections

## Objective

The **Authentication Detections** widget displays the number of failed authentication detections generated by Wazuh over time.

This visualization helps SOC analysts monitor failed login attempts, identify brute-force activity, detect unauthorized authentication attempts, and investigate suspicious login behavior across monitored Windows endpoints.

## Why This Visualization?

Failed authentication events are one of the earliest indicators of password attacks, brute-force attempts, credential guessing, and unauthorized access attempts.

Monitoring authentication failure detections enables SOC analysts to quickly identify abnormal login activity, investigate compromised accounts, and respond to potential attacks before successful access is achieved.

---

## Visualization Configuration

The visualization configuration is identical to **Widget 01 - Process Creation Detections**.

Use the same configuration for:

- Visualization Type
- Data Source
- Y-Axis
- X-Axis
- Save Options

Only the following settings need to be changed.

---

## Step 1 - Update the Filter

Configure the following filter:

**Field**

```text
rule.groups
```

**Operator**

```text
is
```

**Value**

```text
authentication_failed
```

This filter displays only failed authentication detections generated by Windows Security events.

### Screenshot

**Image #19**

```text
dashboards/screenshots/dashboard-02/19-authentication-filter.png
```

---

## Step 2 - Save the Visualization

Save the visualization using the following details.

**Title**

```text
SOC - Failed Authentication Detections
```

**Description**

```text
Displays failed authentication detections generated by Wazuh over time. This visualization helps SOC analysts identify suspicious login attempts, detect brute-force activity, and investigate unauthorized authentication events.
```

Enable:

```text
Add to Dashboards after saving
```

### Screenshot

**Image #20**

```text
dashboards/screenshots/dashboard-02/20-save-authentication-visualization.png
```

---

## Final Result

After saving the visualization, it will be added to the **Enterprise SOC - Windows Endpoint Security Monitoring** dashboard.

The widget provides a timeline of failed authentication detections, enabling SOC analysts to identify abnormal login activity, detect brute-force attempts, and investigate suspicious authentication events across monitored Windows endpoints.

### Screenshot

**Image #21**

```text
dashboards/screenshots/dashboard-02/21-authentication-widget.png
```

## Final Dashboard Documentation

### Dashboard Layout Recommendation

The **Enterprise SOC - Windows Endpoint Security Monitoring** dashboard is designed to provide SOC analysts with centralized visibility into security detections collected from Windows endpoints.

The dashboard groups the most important endpoint detection categories into a single monitoring interface, enabling analysts to quickly identify suspicious activity and begin investigations without navigating multiple Wazuh modules.

### Dashboard Layout

**Endpoint Detection Widgets**

- Process Creation Detections
- PowerShell Detections
- File Creation Detections
- Registry Modification Detections
- Failed Authentication Detections

Each visualization displays detection activity over time, allowing analysts to identify abnormal spikes, correlate endpoint events, and investigate suspicious behavior across monitored Windows systems.

---

The dashboard layout focuses on endpoint telemetry and Windows security monitoring. It complements the **Enterprise SOC - Overview** dashboard by providing detailed visibility into endpoint-related detections while supporting incident investigation and threat analysis.

## Final Enterprise SOC - Windows Endpoint Security Monitoring Dashboard

The completed **Enterprise SOC - Windows Endpoint Security Monitoring** dashboard provides SOC analysts with a centralized view of Windows endpoint security detections generated by Wazuh and Sysmon.

By combining process execution, PowerShell activity, file creation, registry modifications, and failed authentication detections into a single interface, the dashboard enables analysts to quickly identify suspicious endpoint behavior, investigate potential attacks, and improve security monitoring across the Enterprise SOC Lab.

### Screenshot

**Final Enterprise SOC - Windows Endpoint Security Monitoring Dashboard**

```text
dashboards/screenshots/dashboard-02/final-dashboard.png
```

# Final Dashboard

After completing all five visualizations, the **Enterprise SOC - Windows Endpoint Security Monitoring** dashboard provides a centralized view of critical endpoint security detections collected from Windows systems.

The dashboard enables SOC analysts to monitor process execution, PowerShell activity, file creation, registry modifications, and failed authentication attempts in a single location. By visualizing these detections over time, analysts can quickly identify suspicious activity, investigate security events, and respond to potential threats more efficiently.

### Screenshot

**Final Dashboard**

```text
dashboards/screenshots/dashboard-02/final-dashboard.png
```