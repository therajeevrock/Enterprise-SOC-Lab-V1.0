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