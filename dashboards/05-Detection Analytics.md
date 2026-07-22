# Enterprise SOC - Detection Analytics

## Overview

The **Enterprise SOC - Detection Analytics** dashboard provides SOC analysts with analytical insights into Wazuh detection activity across the monitored environment. Rather than focusing on individual security events, this dashboard highlights overall detection trends, rule effectiveness, and alert distribution to help analysts evaluate the performance of security monitoring.

By visualizing detection rules, alert severity, and detection frequency, analysts can quickly identify recurring security events, understand which detection rules generate the most alerts, and recognize changes in detection patterns over time.

This dashboard supports continuous security monitoring, improves detection visibility, and helps security teams prioritize investigation efforts based on alert analytics.

---

# Objective

The objective of this dashboard is to provide a centralized analytical view of Wazuh detection data, enabling SOC analysts to evaluate detection effectiveness, identify frequently triggered detection rules, analyze alert severity, and monitor overall detection activity across the environment.

This dashboard assists analysts in identifying recurring threats, optimizing detection rules, reducing alert fatigue, and improving the overall efficiency of security operations.

---

# Dashboard Information

| Property | Value |
|----------|-------|
| Dashboard Name | Enterprise SOC - Detection Analytics |
| Description | Displays analytical insights into Wazuh detection activity, including detection rules, alert severity, and detection frequency to support SOC monitoring and security analysis. |

---

# Create Dashboard

Navigate to the Wazuh Dashboard.

From the left navigation menu, select:

```text
Dashboards
```

Click:

```text
Create dashboard
```

Configure the dashboard using the following details.

**Dashboard Name**

```text
Enterprise SOC - Detection Analytics
```

**Description**

```text
Displays analytical insights into Wazuh detection activity, including detection rules, alert severity, and detection frequency to support SOC monitoring and security analysis.
```

Click:

```text
Create dashboard
```

### Screenshot

**Image #01**

```text
dashboards/screenshots/dashboard-05/01-create-dashboard.png
```

The new dashboard is created and is ready for adding visualizations.


# Widget 01 - Top Detection Rules

## Objective

The **Top Detection Rules** widget displays the most frequently triggered Wazuh detection rules across monitored endpoints.

This visualization helps SOC analysts identify recurring security events, understand which threats occur most often, and prioritize investigations based on detection frequency.

---

## Why This Visualization?

Security monitoring environments generate numerous alerts every day. Identifying the detection rules that trigger most frequently helps analysts recognize recurring attack patterns, noisy rules, and common security events.

By monitoring the most active detection rules, SOC analysts can quickly identify abnormal behavior, prioritize high-impact threats, and improve the efficiency of incident investigation and response.

---

## Visualization Configuration

### Step 1 - Create a New Visualization

From the dashboard, click:

```text
Create visualization
```

Select the following visualization type:

```text
Horizontal Bar
```

### Screenshot

**Image #02**

```text
dashboards/screenshots/dashboard-05/02-select-horizontal-bar.png
```

---

### Step 2 - Configure the Visualization

Use the following configuration.

**Metric**

```text
Count
```

---

**Buckets**

Aggregation

```text
Terms
```

Field

```text
rule.description
```

Order By

```text
Metric: Count
```

Order

```text
Descending
```

Size

```text
10
```

This configuration displays the ten most frequently triggered Wazuh detection rules based on the number of generated alerts.

### Screenshot

**Image #03**

```text
dashboards/screenshots/dashboard-05/03-top-detection-rules-configuration.png
```

---

### Step 3 - Save the Visualization

Save the visualization using the following details.

**Title**

```text
SOC - Top Detection Rules
```

**Description**

```text
Displays the most frequently triggered Wazuh detection rules. This visualization helps SOC analysts identify recurring security events, prioritize investigations, and recognize common attack patterns across monitored endpoints.
```

Enable:

```text
Add to Dashboards after saving
```

### Screenshot

**Image #04**

```text
dashboards/screenshots/dashboard-05/04-save-top-detection-rules.png
```

---

## Final Result

After saving the visualization, it is added to the **Enterprise SOC - Detection Analytics** dashboard.

The widget displays the most frequently triggered Wazuh detection rules, allowing SOC analysts to quickly identify recurring security events, prioritize investigations, and understand which detections require the most attention.

### Screenshot

**Image #05**

```text
dashboards/screenshots/dashboard-05/05-top-detection-rules-widget.png
```

---

# Widget 02 - Rule Level Distribution

## Objective

The **Rule Level Distribution** widget displays the distribution of Wazuh detection rules based on their severity levels during the selected time range.

This visualization helps SOC analysts understand the overall severity of generated security alerts, identify the proportion of low- and high-priority detections, and evaluate the security posture of the monitored environment.

---

## Why This Visualization?

Not all security alerts have the same level of importance. Wazuh assigns a severity level to every detection rule, allowing analysts to prioritize investigations based on the potential impact of an event.

By visualizing the distribution of rule severity levels, SOC analysts can quickly determine whether the environment is experiencing mostly informational events or a significant number of high-severity detections requiring immediate attention.

This visualization supports alert prioritization, incident triage, and overall security monitoring.

---

## Visualization Configuration

### Step 1 - Create a New Visualization

From the dashboard, click:

```text
Create visualization
```

Select the following visualization type:

```text
Pie
```

### Screenshot

**Image #06**

```text
dashboards/screenshots/dashboard-05/06-select-pie-chart.png
```

---

### Step 2 - Configure the Visualization

Use the following configuration.

**Metric**

```text
Count
```

---

**Buckets**

Aggregation

```text
Terms
```

Field

```text
rule.level
```

Order By

```text
Metric: Count
```

Order

```text
Descending
```

Size

```text
16
```

This configuration groups security alerts according to their Wazuh rule severity levels and displays the distribution of alerts across each severity category.

### Screenshot

**Image #07**

```text
dashboards/screenshots/dashboard-05/07-rule-level-distribution-configuration.png
```

---

### Step 3 - Save the Visualization

Save the visualization using the following details.

**Title**

```text
SOC - Rule Level Distribution
```

**Description**

```text
Displays the distribution of Wazuh rule severity levels across generated security alerts, helping SOC analysts prioritize investigations and assess the overall alert severity within the monitored environment.
```

Enable:

```text
Add to Dashboards after saving
```

### Screenshot

**Image #08**

```text
dashboards/screenshots/dashboard-05/08-save-rule-level-distribution.png
```

---

## Final Result

After saving the visualization, it is added to the **Enterprise SOC - Detection Analytics** dashboard.

The widget displays the distribution of Wazuh rule severity levels, enabling SOC analysts to quickly evaluate the proportion of low-, medium-, and high-severity alerts, prioritize incident response efforts, and monitor the overall security posture of the environment.

### Screenshot

**Image #09**

```text
dashboards/screenshots/dashboard-05/09-rule-level-distribution-widget.png
```

---

# Widget 03 - Top Rule IDs

## Objective

The **Top Rule IDs** widget displays the Wazuh Rule IDs that generated the highest number of security alerts during the selected time range.

This visualization helps SOC analysts identify the most frequently triggered detection rules using their unique Rule IDs, enabling faster rule correlation, investigation, and detection analysis.

---

## Why This Visualization?

Each Wazuh detection rule is assigned a unique Rule ID that serves as its permanent identifier. During security investigations, analysts frequently reference Rule IDs when reviewing alerts, analyzing detection logic, tuning rules, or consulting official Wazuh documentation.

Displaying the most frequently triggered Rule IDs allows SOC analysts to quickly identify which detection rules are generating the highest alert volume and supports efficient incident investigation and rule management.

---

## Visualization Configuration

### Step 1 - Create a New Visualization

From the dashboard, click:

```text
Create visualization
```

Select the following visualization type:

```text
Data Table
```

### Screenshot

**Image #10**

```text
dashboards/screenshots/dashboard-05/10-select-data-table.png
```

---

### Step 2 - Configure the Visualization

Use the following configuration.

**Metric**

```text
Count
```

---

**Buckets**

Aggregation

```text
Terms
```

Field

```text
rule.id
```

Order By

```text
Metric: Count
```

Order

```text
Descending
```

Size

```text
10
```

This configuration displays the ten most frequently triggered Wazuh Rule IDs along with the number of generated alerts for each rule.

### Screenshot

**Image #11**

```text
dashboards/screenshots/dashboard-05/11-top-rule-ids-configuration.png
```

---

### Step 3 - Save the Visualization

Save the visualization using the following details.

**Title**

```text
SOC - Top Rule IDs
```

**Description**

```text
Displays the most frequently triggered Wazuh Rule IDs and their alert counts, helping SOC analysts identify recurring detection rules and support investigation and rule analysis.
```

Enable:

```text
Add to Dashboards after saving
```

### Screenshot

**Image #12**

```text
dashboards/screenshots/dashboard-05/12-save-top-rule-ids.png
```

---

## Final Result

After saving the visualization, it is added to the **Enterprise SOC - Detection Analytics** dashboard.

The widget displays the most frequently triggered Wazuh Rule IDs and their corresponding alert counts, enabling SOC analysts to quickly correlate alerts with specific detection rules, investigate recurring security events, and support rule tuning and threat analysis.

### Screenshot

**Image #13**

```text
dashboards/screenshots/dashboard-05/13-top-rule-ids-widget.png
```

---

# Widget 04 - Top Rule Groups

## Objective

The **Top Rule Groups** widget displays the Wazuh rule groups that generated the highest number of security alerts during the selected time range.

This visualization helps SOC analysts identify the security categories responsible for the largest volume of alerts, providing a high-level understanding of detection activity across the monitored environment.

---

## Why This Visualization?

Wazuh organizes detection rules into rule groups that represent different security categories, such as Windows events, Sysmon activity, authentication events, malware detections, and compliance frameworks.

Monitoring the most active rule groups enables SOC analysts to quickly identify which categories are generating the highest alert volume. This helps prioritize investigations, recognize recurring attack patterns, and evaluate which areas of the environment require additional attention.

Unlike individual Rule IDs or Rule Descriptions, Rule Groups provide a broader analytical view of detection activity.

---

## Visualization Configuration

### Step 1 - Create a New Visualization

From the dashboard, click:

```text
Create visualization
```

Select the following visualization type:

```text
Horizontal Bar
```

### Screenshot

**Image #14**

```text
dashboards/screenshots/dashboard-05/14-select-horizontal-bar.png
```

---

### Step 2 - Configure the Visualization

Use the following configuration.

**Metric**

```text
Count
```

---

**Buckets**

Aggregation

```text
Terms
```

Field

```text
rule.groups
```

Order By

```text
Metric: Count
```

Order

```text
Descending
```

Size

```text
10
```

This configuration groups security alerts by their associated Wazuh rule groups and displays the ten most frequently occurring categories based on alert count.

### Screenshot

**Image #15**

```text
dashboards/screenshots/dashboard-05/15-top-rule-groups-configuration.png
```

---

### Step 3 - Save the Visualization

Save the visualization using the following details.

**Title**

```text
SOC - Top Rule Groups
```

**Description**

```text
Displays the most frequently occurring Wazuh rule groups, helping SOC analysts identify the security categories generating the highest number of alerts across the monitored environment.
```

Enable:

```text
Add to Dashboards after saving
```

### Screenshot

**Image #16**

```text
dashboards/screenshots/dashboard-05/16-save-top-rule-groups.png
```

---

## Final Result

After saving the visualization, it is added to the **Enterprise SOC - Detection Analytics** dashboard.

The widget displays the most frequently occurring Wazuh rule groups, enabling SOC analysts to quickly identify dominant security event categories, understand overall detection trends, and prioritize investigations based on alert categories rather than individual rules.

### Screenshot

**Image #17**

```text
dashboards/screenshots/dashboard-05/17-top-rule-groups-widget.png
```

---

# Final Dashboard Documentation

The **Enterprise SOC - Detection Analytics** dashboard provides SOC analysts with a centralized analytical view of Wazuh detection activity across the monitored environment. Unlike dashboards focused on real-time monitoring or threat investigation, this dashboard emphasizes detection analytics by highlighting the most active detection rules, alert severity distribution, frequently triggered Rule IDs, and the security categories generating the highest number of alerts.

The dashboard combines four visualizations that provide complementary insights into detection performance and alert behavior:

- **Top Detection Rules** identifies the detection rules that generate the highest number of security alerts.
- **Rule Level Distribution** displays the distribution of alerts across Wazuh severity levels, helping analysts prioritize incidents based on risk.
- **Top Rule IDs** lists the most frequently triggered Wazuh Rule IDs, enabling analysts to quickly correlate alerts with specific detection logic and official Wazuh documentation.
- **Top Rule Groups** highlights the security categories that produce the highest alert volume, providing a broader understanding of detection activity across the environment.

Together, these visualizations help SOC analysts:

- Identify the most frequently triggered detection rules.
- Analyze the distribution of alert severity levels.
- Correlate alerts using Wazuh Rule IDs.
- Understand which security categories generate the highest number of alerts.
- Support rule tuning, detection engineering, threat hunting, and incident response.
- Improve overall visibility into the effectiveness of security monitoring.

This dashboard enables SOC analysts to evaluate detection performance, recognize recurring security events, optimize detection strategies, and make informed decisions based on comprehensive detection analytics.

---

# Final Dashboard

After adding all visualizations, the completed dashboard contains the following widgets:

| Widget | Visualization |
|---------|---------------|
| Top Detection Rules | Horizontal Bar Chart |
| Rule Level Distribution | Pie Chart |
| Top Rule IDs | Data Table |
| Top Rule Groups | Horizontal Bar Chart |

### Screenshot

**Image #18**

```text
dashboards/screenshots/dashboard-05/18-final-dashboard.png
```