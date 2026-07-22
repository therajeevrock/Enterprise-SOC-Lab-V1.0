# Dashboard 04 - MITRE ATT&CK Analysis

## Overview

The **Enterprise SOC - MITRE ATT&CK Analysis** dashboard provides SOC analysts with a centralized view of adversary techniques and tactics identified by the Wazuh Rule Engine.

Unlike the previous dashboards, which focus on overall security posture, endpoint monitoring, and threat detection, this dashboard is dedicated to analyzing MITRE ATT&CK mappings. It enables analysts to understand attacker behavior, identify commonly observed techniques, evaluate detection coverage, and improve threat hunting activities.

This dashboard serves as the primary workspace for analyzing MITRE ATT&CK techniques and tactics within the Enterprise SOC Lab.

## Objective

- Monitor MITRE ATT&CK techniques detected by Wazuh.
- Analyze adversary tactics and techniques associated with security alerts.
- Identify the most frequently observed attacker behaviors.
- Evaluate detection coverage across the MITRE ATT&CK framework.
- Support threat hunting and incident investigations using MITRE ATT&CK mappings.

---

## Dashboard Information

| Property | Value |
|----------|-------|
| Dashboard Name | Enterprise SOC - MITRE ATT&CK Analysis |
| Platform | Wazuh Dashboard |
| Visualization Engine | OpenSearch Dashboards |
| Data Source | wazuh-alerts-* |
| Time Filter | Last 24 Hours |

---

# Create Dashboard

## Objective

Create a dedicated dashboard for analyzing MITRE ATT&CK techniques and tactics detected by Wazuh. This dashboard centralizes MITRE ATT&CK visualizations, enabling SOC analysts to understand attacker behavior, evaluate detection coverage, support threat hunting, and improve incident response through framework-based analysis.


# Create Dashboard

## Objective

Create a dedicated dashboard for analyzing MITRE ATT&CK techniques and tactics detected by the Wazuh Rule Engine. This dashboard centralizes MITRE ATT&CK visualizations, enabling SOC analysts to understand attacker behavior, evaluate detection coverage, support threat hunting, and improve incident response through framework-based analysis.

---

## Step 1 - Open the Dashboards Page

From the Wazuh Dashboard navigation menu:

```text
Menu
→ Dashboards
```

Click **Create dashboard**.

### Screenshot

**Image #01**

```text
dashboards/screenshots/dashboard-04/01-open-create-dashboard.png
```

---

## Step 2 - Configure Dashboard Details

Provide the following information.

### Title

```text
Enterprise SOC - MITRE ATT&CK Analysis
```

### Description

```text
The MITRE ATT&CK Analysis dashboard provides SOC analysts with a centralized view of adversary techniques and tactics detected by Wazuh. It helps identify attacker behavior, analyze MITRE ATT&CK mappings, evaluate detection coverage, and support threat hunting and incident investigations through framework-based security insights.
```

Enable the following option:

```text
Store time with dashboard
```

Click **Save** to create the dashboard.

### Screenshot

**Image #02**

```text
dashboards/screenshots/dashboard-04/02-dashboard-details.png
```

---

## Result

A new dashboard named **Enterprise SOC - MITRE ATT&CK Analysis** is successfully created and is ready for adding MITRE ATT&CK visualizations.

### Screenshot

**Image #03**

```text
dashboards/screenshots/dashboard-04/03-empty-dashboard.png
```

---

# Widget 01 - Top MITRE ATT&CK Techniques

## Objective

The **Top MITRE ATT&CK Techniques** widget displays the MITRE ATT&CK techniques that generated the highest number of security alerts during the selected time range.

This visualization helps SOC analysts understand attacker behavior, identify commonly observed techniques, evaluate detection coverage, and prioritize threat hunting and incident investigations.

---

## Why This Visualization?

MITRE ATT&CK techniques represent the specific methods used by attackers to achieve their objectives during different stages of an attack.

Monitoring the most frequently detected techniques enables SOC analysts to understand which adversary behaviors are most commonly observed within the environment, evaluate detection effectiveness, and identify areas that may require additional monitoring or security controls.

By visualizing the top MITRE ATT&CK techniques, analysts can quickly recognize attack patterns without manually reviewing individual security alerts.

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

A Horizontal Bar chart provides better readability for MITRE ATT&CK technique names and makes it easier to compare the most frequently detected techniques.

### Screenshot

**Image #04**

```text
dashboards/screenshots/dashboard-04/04-select-horizontal-bar.png
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
rule.mitre.technique
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

This configuration groups security alerts by their associated MITRE ATT&CK technique and displays the ten most frequently detected techniques during the selected time range.

### Screenshot

**Image #05**

```text
dashboards/screenshots/dashboard-04/05-configure-top-mitre-techniques.png
```

---

### Step 3 - Save the Visualization

Save the visualization using the following details.

**Title**

```text
SOC - Top MITRE ATT&CK Techniques
```

**Description**

```text
Displays the top 10 MITRE ATT&CK techniques associated with security alerts. This visualization helps SOC analysts identify the most frequently observed attacker behaviors and evaluate detection coverage.
```

Enable:

```text
Add to Dashboards after saving
```

### Screenshot

**Image #06**

```text
dashboards/screenshots/dashboard-04/06-save-top-mitre-techniques.png
```

---

## Final Result

After saving the visualization, it is added to the **Enterprise SOC - MITRE ATT&CK Analysis** dashboard.

The widget displays the most frequently detected MITRE ATT&CK techniques, enabling SOC analysts to understand attacker behavior, recognize recurring attack patterns, evaluate detection coverage, and support threat hunting activities.

### Screenshot

**Image #07**

```text
dashboards/screenshots/dashboard-04/07-top-mitre-techniques-widget.png
```