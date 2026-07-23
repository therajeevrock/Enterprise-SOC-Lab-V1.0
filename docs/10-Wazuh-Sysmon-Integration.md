# Wazuh–Sysmon Integration

---

# Chapter Information

| Property                   | Details                                  |
|----------------------------|------------------------------------------|
| **Chapter Number**         | 10                                       |
| **Chapter Title**          | Wazuh–Sysmon Integration                 |
| **Difficulty**             | Intermediate                             |
| **Estimated Reading Time** | 15–20 Minutes                            |
| **Estimated Lab Time**     | 20–30 Minutes                            |
| **Prerequisite**           | Chapter 09 – Sysmon Installation         |
| **Category**               | Integration Guide                        | 
| **Related Troubleshooting**| Troubleshooting.md                       |
| **Next Chapter**           | Chapter 11 – Security Event Verification |

---

# Document Information

| Property          | Details                  |
|-------------------|--------------------------|
| **Project**       | Enterprise SOC Lab       |
| **Document Name** | Wazuh–Sysmon Integration |
| **Version**       | 1.0                      |
| **Author**        | Rajeev Rock              |
| **Status**        | Published                |
| **Last Updated**  | July 2026                |

---

# Learning Objectives

After completing this chapter, you will be able to:

- Understand how Wazuh collects Sysmon events.
- Configure the Wazuh Agent to monitor the Sysmon Operational log.
- Restart the Wazuh Agent after updating the configuration.
- Verify that Sysmon events are forwarded to the Wazuh Manager.
- Confirm that Sysmon events are visible in the Wazuh Dashboard.
- Prepare the lab for security monitoring and event analysis.

---

# Objective

The objective of this chapter is to integrate Microsoft Sysmon with the Wazuh platform.

After installing Sysmon, Windows begins generating detailed endpoint telemetry. However, these events are not automatically collected by Wazuh.

To enable security monitoring, the Wazuh Agent must be configured to read the **Microsoft-Windows-Sysmon/Operational** event channel and forward the collected events to the Wazuh Manager.

Once the integration is complete, Sysmon events become searchable in the Wazuh Dashboard and can be used for alert generation, threat detection, and security investigations.

This chapter focuses only on configuring the integration and verifying successful event collection. Event analysis, detection rules, and SOC investigations will be covered in later chapters.

---

# How Wazuh Collects Sysmon Events

The following diagram illustrates the data flow after Sysmon has been integrated with the Wazuh platform.

```text
Sysmon
      │
      ▼
Windows Event Log
      │
      ▼
Wazuh Agent
      │
      ▼
Wazuh Manager
      │
      ▼
OpenSearch Indexer
      │
      ▼
Wazuh Dashboard
```

The Wazuh Agent continuously monitors the Sysmon Operational event log on the Windows endpoint. Whenever Sysmon records a new event, the agent forwards it to the Wazuh Manager. The Manager processes the event, stores it in the OpenSearch Indexer, and makes it available through the Wazuh Dashboard for monitoring and investigation.

---

# Integration Prerequisites

Before configuring the integration, verify that the following requirements have been completed.

| Requirement                                          | Status |
|------------------------------------------------------|--------|
| Wazuh Platform installed                             | ✓      |
| Windows 10 endpoint deployed                         | ✓      |
| Wazuh Agent installed and connected                  | ✓      |
| Sysmon installed successfully                        | ✓      |
| Sysmon Operational log available                     | ✓      |
| Windows endpoint communicating with the Wazuh Manager| ✓      |

Completing these prerequisites ensures that the integration can be configured successfully without additional installation steps.

---

# Configure the Wazuh Agent

To enable Sysmon event collection, the Wazuh Agent must be configured to monitor the **Microsoft-Windows-Sysmon/Operational** event channel.

The configuration is performed by editing the Wazuh Agent configuration file.

Configuration File:

```text
C:\Program Files (x86)\ossec-agent\ossec.conf
```

> **📸 Screenshot Required**
>
> **Title:** Wazuh Agent Configuration File
>
> **Save As:** `screenshots/integration/01-ossec-conf.png`

---

# Configure Sysmon Log Collection

Open the `ossec.conf` file using a text editor with administrative privileges.

Add the following configuration inside the `<ossec_config>` section.

```xml
<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>
```

This configuration instructs the Wazuh Agent to monitor the Sysmon Operational event channel and forward all collected events to the Wazuh Manager.

Save the configuration file after making the changes.

> **📸 Screenshot Required**
>
> **Title:** Sysmon Event Channel Configuration
>
> **Save As:** `screenshots/integration/02-sysmon-eventchannel.png`

---

# Restart the Wazuh Agent

After updating the configuration, restart the Wazuh Agent service to apply the changes.

Open **Command Prompt** as **Administrator** and run:

```cmd
net stop WazuhSvc
```

```cmd
net start WazuhSvc
```

Alternatively, the service can be restarted from the **Windows Services** console.

After the restart, the Wazuh Agent begins monitoring the Sysmon Operational event log.

> **📸 Screenshot Required**
>
> **Title:** Restarting the Wazuh Agent
>
> **Save As:** `screenshots/integration/03-agent-restart.png`

---

# Verify the Configuration

Open the `ossec.conf` file again and confirm that:

- The Sysmon event channel has been added.
- The XML structure is valid.
- The configuration file has been saved successfully.
- The Wazuh Agent service restarted without errors.

At this stage, the integration has been configured successfully.

The next step is to generate Sysmon events and verify that they are visible in the Wazuh Dashboard.

---

# Generate Test Events

After configuring the Wazuh Agent, generate a few test events on the Windows 10 endpoint.

These events will verify that Sysmon is collecting endpoint telemetry and that the Wazuh Agent is forwarding the events to the Wazuh platform.

For this verification, perform one or more of the following actions:

- Open **Notepad**
- Open **Command Prompt**
- Open **PowerShell**
- Create a new folder
- Launch any Windows application

These actions generate Sysmon events that can later be verified in the Wazuh Dashboard.

> **📸 Screenshot Required**
>
> **Title:** Generate Test Events
>
> **Save As:** `screenshots/integration/04-generate-test-events`

---

# Verify Events in the Wazuh Dashboard

Open the Wazuh Dashboard from the Windows 11 host machine.

Navigate to:

```text
Wazuh
    ↓
Events
```

Search for recent events generated from the Windows 10 endpoint.

Verify that new Sysmon events appear in the dashboard after performing the test activities.

> **📸 Screenshot Required**
>
> **Title:** Sysmon Events in Wazuh Dashboard
>
> **Save As:** `screenshots/integration/05-dashboard-events.png`

---

# Verify Event Collection

Select one of the generated events and review its basic information.

Confirm that the event contains details such as:

- Agent Name
- Hostname
- Event Timestamp
- Event Provider (Sysmon)
- Event ID
- Process Name

The presence of this information confirms that Sysmon events are being collected successfully by the Wazuh platform.

> **📸 Screenshot Required**
>
> **Title:** Sysmon Event Details
>
> **Save As:** `screenshots/integration/06-event-details.png`

---

# Verify End-to-End Integration

The final verification is to confirm that the complete monitoring pipeline is functioning correctly.

```text
Windows Activity
        │
        ▼
Sysmon
        │
        ▼
Windows Event Log
        │
        ▼
Wazuh Agent
        │
        ▼
Wazuh Manager
        │
        ▼
OpenSearch Indexer
        │
        ▼
Wazuh Dashboard
```

If the generated events appear in the Wazuh Dashboard, the integration has been completed successfully.

At this stage, the Enterprise SOC Lab is capable of collecting Windows endpoint telemetry and preparing it for security monitoring and future investigations.

> **📸 Screenshot Required**
>
> **Title:** End-to-End Integration Verification
>
> **Save As:** `screenshots/integration/07-end-to-end-verification.png`

---

# Integration Checklist

Before proceeding to the next chapter, verify that the Sysmon integration has been completed successfully.

| Status | Task |
|:------:|------|
| ☐ | Sysmon installed successfully |
| ☐ | Wazuh Agent configuration updated |
| ☐ | Sysmon Operational log added to `ossec.conf` |
| ☐ | Wazuh Agent restarted successfully |
| ☐ | Sysmon events generated |
| ☐ | Events visible in the Wazuh Dashboard |
| ☐ | End-to-end event flow verified |
| ☐ | Enterprise SOC Lab is ready for security monitoring |

---

# Common Integration Issues

### 1. Sysmon events do not appear in the Wazuh Dashboard

**Possible Cause**

- Sysmon event channel is not configured in `ossec.conf`.
- The Wazuh Agent has not been restarted.

**Resolution**

- Verify the `ossec.conf` configuration.
- Restart the Wazuh Agent.
- Generate new test events.

---

### 2. Wazuh Agent is not forwarding Sysmon events

**Possible Cause**

- Incorrect XML configuration.
- Wazuh Agent service is not running.

**Resolution**

- Review the XML configuration.
- Confirm the Wazuh Agent service is running.
- Restart the service if required.

---

### 3. Sysmon Operational log is empty

**Possible Cause**

- Sysmon is not installed correctly.
- No Windows activity has been generated.

**Resolution**

- Verify the Sysmon service.
- Generate test activity such as launching Notepad or Command Prompt.
- Refresh the Event Viewer.

---

### 4. Dashboard shows no recent events

**Possible Cause**

- Communication issue between the Windows endpoint and the Wazuh Manager.
- Network connectivity problem.

**Resolution**

- Verify network connectivity.
- Confirm that the Windows endpoint is connected to the Wazuh Manager.
- Check the Wazuh Dashboard for agent status.

> **Need Help?**
>
> Refer to **Troubleshooting.md** for detailed troubleshooting procedures, command references, and real-world issues encountered while building this Enterprise SOC Lab.

---

# Summary

In this chapter, Microsoft Sysmon was successfully integrated with the Wazuh platform.

The Wazuh Agent was configured to collect Sysmon Operational events, forward them to the Wazuh Manager, and make them available through the Wazuh Dashboard.

The successful verification of event collection confirms that the Enterprise SOC Lab is fully operational and capable of collecting detailed Windows endpoint telemetry for security monitoring.

---

# Key Takeaways

After completing this chapter, you can:

- Configure the Wazuh Agent to collect Sysmon events.
- Monitor the Sysmon Operational event channel.
- Verify end-to-end event forwarding.
- Confirm Sysmon event visibility in the Wazuh Dashboard.
- Prepare the lab for SOC investigations and log analysis.

---

# Next Chapter

The next chapter introduces **Understanding Sysmon**.

Rather than focusing on installation, it explains how Sysmon generates Windows telemetry, why specific events are important, and how SOC analysts use Sysmon logs during investigations.

**Topics Covered**

- What Sysmon monitors
- Understanding Sysmon Event IDs
- Common Sysmon events
- Sysmon telemetry
- SOC use cases
- Best practices

**Next Document**

`11 - Understanding-Sysmon.md`

---

# Document Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | July 2026 | Rajeev Rock | Initial release of the Wazuh–Sysmon Integration guide. |

