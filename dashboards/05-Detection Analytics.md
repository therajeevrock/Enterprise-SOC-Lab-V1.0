
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

**Image #04**

```text
dashboards/screenshots/dashboard-03/04-select-horizontal-bar.png
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

**Image #05**

```text
dashboards/screenshots/dashboard-03/05-top-detection-rules-configuration.png
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

**Image #06**

```text
dashboards/screenshots/dashboard-03/06-save-top-detection-rules.png
```

---

## Final Result

After saving the visualization, it is added to the **Enterprise SOC - Threat Detection & MITRE ATT&CK Overview** dashboard.

The widget displays the most frequently triggered Wazuh detection rules, allowing SOC analysts to quickly identify recurring security events, prioritize investigations, and understand which detections require the most attention.

### Screenshot

**Image #07**

```text
dashboards/screenshots/dashboard-03/07-top-detection-rules-widget.png
```
