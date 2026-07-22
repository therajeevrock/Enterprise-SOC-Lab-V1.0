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

---

# Widget 02 - Top MITRE ATT&CK Tactics

## Objective

The **Top MITRE ATT&CK Tactics** widget displays the MITRE ATT&CK tactics that generated the highest number of security alerts during the selected time range.

This visualization helps SOC analysts understand the primary stages of the attack lifecycle, identify the most frequently observed adversary objectives, and improve threat hunting and incident investigation.

---

## Why This Visualization?

MITRE ATT&CK tactics represent the adversary's tactical objectives during different stages of an attack, such as Initial Access, Execution, Persistence, Privilege Escalation, Defense Evasion, Credential Access, Discovery, Lateral Movement, Collection, Exfiltration, and Impact.

Monitoring the most frequently detected tactics enables SOC analysts to quickly understand which phases of the attack lifecycle are most active within the environment, evaluate detection coverage, and prioritize security investigations.

By visualizing MITRE ATT&CK tactics, analysts gain a high-level understanding of attacker objectives without reviewing individual security alerts.

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

A Horizontal Bar chart provides better readability for MITRE ATT&CK tactic names and makes it easier to compare the most frequently detected tactics.

### Screenshot

**Image #08**

```text
dashboards/screenshots/dashboard-04/08-select-horizontal-bar.png
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
rule.mitre.tactic
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

This configuration groups security alerts by their associated MITRE ATT&CK tactic and displays the ten most frequently detected tactics during the selected time range.

### Screenshot

**Image #09**

```text
dashboards/screenshots/dashboard-04/09-configure-top-mitre-tactics.png
```

---

### Step 3 - Save the Visualization

Save the visualization using the following details.

**Title**

```text
SOC - Top MITRE ATT&CK Tactics
```

**Description**

```text
Displays the top 10 MITRE ATT&CK tactics associated with security alerts. This visualization helps SOC analysts identify the most frequently observed adversary objectives and evaluate detection coverage.
```

Enable:

```text
Add to Dashboards after saving
```

### Screenshot

**Image #10**

```text
dashboards/screenshots/dashboard-04/10-save-top-mitre-tactics.png
```

---

## Final Result

After saving the visualization, it is added to the **Enterprise SOC - MITRE ATT&CK Analysis** dashboard.

The widget displays the most frequently detected MITRE ATT&CK tactics, enabling SOC analysts to understand attacker objectives, identify active phases of the attack lifecycle, evaluate detection coverage, and support threat hunting and incident investigations.

### Screenshot

**Image #11**

```text
dashboards/screenshots/dashboard-04/11-top-mitre-tactics-widget.png
```

---

# Widget 03 - Top MITRE ATT&CK Technique IDs

## Objective

The **Top MITRE ATT&CK Technique IDs** widget displays the MITRE ATT&CK Technique IDs that generated the highest number of security alerts during the selected time range.

This visualization helps SOC analysts quickly identify the most frequently detected MITRE ATT&CK Technique IDs, correlate alerts with the MITRE ATT&CK framework, and support threat hunting and incident investigations.

---

## Why This Visualization?

Each MITRE ATT&CK Technique is assigned a unique identifier (for example, **T1059**, **T1547**, or **T1003**) that is widely used across security reports, threat intelligence platforms, and incident response documentation.

Monitoring the most frequently detected Technique IDs enables SOC analysts to quickly correlate security alerts with the MITRE ATT&CK framework, understand attacker behavior, and investigate related techniques without manually reviewing individual alerts.

This visualization provides a standardized view of attacker techniques, making it easier to communicate findings, perform threat hunting, and reference MITRE ATT&CK documentation during investigations.

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

**Image #12**

```text
dashboards/screenshots/dashboard-04/12-select-pie-chart.png
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
rule.mitre.id
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

This configuration groups security alerts by their associated MITRE ATT&CK Technique ID and displays the ten most frequently detected Technique IDs during the selected time range.

### Screenshot

**Image #13**

```text
dashboards/screenshots/dashboard-04/13-top-mitre-technique-ids-configuration.png
```

---

### Step 3 - Save the Visualization

Save the visualization using the following details.

**Title**

```text
SOC - Top MITRE ATT&CK Technique IDs
```

**Description**

```text
Displays the top 10 MITRE ATT&CK Technique IDs associated with security alerts. This visualization helps SOC analysts identify the most frequently detected attacker techniques and correlate alerts with the MITRE ATT&CK framework.
```

Enable:

```text
Add to Dashboards after saving
```

### Screenshot

**Image #14**

```text
dashboards/screenshots/dashboard-04/14-save-top-mitre-technique-ids.png
```

---

## Final Result

After saving the visualization, it is added to the **Enterprise SOC - MITRE ATT&CK Analysis** dashboard.

The widget displays the most frequently detected MITRE ATT&CK Technique IDs, enabling SOC analysts to quickly correlate security alerts with the MITRE ATT&CK framework, identify recurring attacker techniques, and support threat hunting and incident investigations.

### Screenshot

**Image #15**

```text
dashboards/screenshots/dashboard-04/15-top-mitre-technique-ids-widget.png
```

---

# Final Dashboard Documentation

The **Enterprise SOC - MITRE ATT&CK Analysis** dashboard provides SOC analysts with a centralized view of MITRE ATT&CK mappings generated by Wazuh security alerts. By organizing detections according to the MITRE ATT&CK framework, the dashboard enables analysts to understand attacker behavior, identify the stages of an attack, and correlate security events with known adversary techniques.

The dashboard includes three visualizations that present different perspectives of MITRE ATT&CK data:

- **Top MITRE ATT&CK Techniques** identifies the most frequently detected attacker techniques observed in the environment.
- **Top MITRE ATT&CK Tactics** highlights the attack phases most commonly associated with security alerts.
- **Top MITRE ATT&CK Technique IDs** displays the MITRE ATT&CK Technique IDs that occur most frequently, allowing analysts to quickly correlate detections with the MITRE ATT&CK framework and supporting documentation.

Together, these visualizations help SOC analysts:

- Monitor attacker behavior using the MITRE ATT&CK framework.
- Identify the most frequently observed attack techniques and tactics.
- Correlate alerts with standardized MITRE ATT&CK Technique IDs.
- Support threat hunting and incident investigations.
- Improve security reporting and communication using a globally recognized attack framework.

This dashboard provides valuable operational visibility into adversary behavior and helps security teams investigate, prioritize, and respond to threats more effectively.

---

# Final Dashboard

After adding all visualizations, the completed dashboard contains the following widgets:

| Widget | Visualization |
|---------|---------------|
| Top MITRE ATT&CK Techniques | Horizontal Bar Chart |
| Top MITRE ATT&CK Tactics | Horizontal Bar Chart |
| Top MITRE ATT&CK Technique IDs | Pie Chart |

### Screenshot

**Image #16**

```text
dashboards/screenshots/dashboard-04/16-final-dashboard.png
```