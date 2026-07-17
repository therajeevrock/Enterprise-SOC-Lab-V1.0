# Understanding Wazuh

---

# Chapter Information

| Property | Details |
|----------|---------|
| **Chapter Number** | 12 |
| **Chapter Title** | Understanding Wazuh |
| **Difficulty** | Beginner → Intermediate |
| **Estimated Reading Time** | 10–15 Minutes |
| **Estimated Lab Time** | N/A (Theory Chapter) |
| **Prerequisite** | Chapter 11 – Understanding Sysmon |
| **Category** | Concept Guide |
| **Next Chapter** | Chapter 13 – Understanding Logs |

---

# Document Information

| Property | Details |
|----------|---------|
| **Project** | Enterprise SOC Lab |
| **Document Name** | Understanding Wazuh |
| **Version** | 1.0 |
| **Author** | Rajeev Kumar |
| **Status** | Published |
| **Last Updated** | July 2026 |

---

# Learning Objectives

After completing this chapter, you will be able to:

- Understand the purpose of Wazuh.
- Identify the major components of the Wazuh platform.
- Learn how Wazuh processes security events.
- Understand the role of Wazuh in a Security Operations Center (SOC).
- Prepare for the upcoming log analysis chapters.

---

# Objective

This chapter explains the role of Wazuh within the Enterprise SOC Lab.

The installation and configuration of Wazuh were completed in the previous chapters. This chapter focuses on understanding how Wazuh receives, processes, stores, and presents security events for monitoring and investigation.

> **Important**
>
> This chapter is directly related to **Chapter 07 – Wazuh Installation**.
>
> If Wazuh has not been installed and configured successfully, complete **Chapter 07** before continuing.

---

# What is Wazuh?

Wazuh is an open-source security platform used for endpoint monitoring, threat detection, log analysis, file integrity monitoring, vulnerability detection, and incident response.

It collects security events from monitored endpoints through the Wazuh Agent and processes them on the Wazuh platform.

Within this Enterprise SOC Lab, Wazuh acts as the central platform that receives telemetry from the Windows endpoint and presents it through the Wazuh Dashboard.

---

# How Does Wazuh Work?

The overall workflow is straightforward.

```text
Windows Endpoint
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

The Wazuh Agent collects events from the monitored endpoint and securely forwards them to the Wazuh Manager.

The Manager analyzes the events, stores them in the OpenSearch Indexer, and makes them available in the Wazuh Dashboard for monitoring and investigation.

---

# Major Components of Wazuh

The Wazuh platform consists of several components that work together to collect, process, store, and display security events.

| Component | Purpose |
|----------|---------|
| Wazuh Agent | Collects security events from monitored endpoints. |
| Wazuh Manager | Receives, analyzes, and processes events from agents. |
| OpenSearch Indexer | Stores processed events for searching and analysis. |
| Wazuh Dashboard | Provides a web interface for monitoring, investigations, and visualization. |

Each component has a specific responsibility, and together they form the complete Wazuh platform.

---

# What Does Wazuh Monitor?

Wazuh collects security-related information from monitored systems.

Some common examples include:

- Windows Event Logs
- Sysmon Events
- Authentication Events
- File Integrity Changes
- Security Alerts
- System Information

The collected data helps security analysts monitor endpoint activity and investigate suspicious behavior.

---

# Why Do SOC Analysts Use Wazuh?

Wazuh provides centralized visibility into multiple endpoints from a single dashboard.

Some common use cases include:

- Monitoring endpoint activity
- Investigating security alerts
- Detecting suspicious behavior
- Supporting incident response
- Collecting logs for forensic analysis

Instead of reviewing each endpoint individually, analysts can investigate events through a centralized platform.

---

# Wazuh Data Flow

The simplified workflow of Wazuh is shown below.

```text
Endpoint
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

This workflow enables security events generated on an endpoint to be collected, processed, stored, and displayed for analysis.

---

# How Wazuh Supports Security Monitoring

Wazuh helps security analysts monitor endpoint activity by collecting and organizing security events from multiple systems.

Rather than manually reviewing logs on individual machines, analysts can use the Wazuh Dashboard to search, filter, and investigate security events from a centralized location.

This centralized approach improves visibility and reduces the time required to detect and investigate suspicious activity.

---

# How Wazuh Works with Sysmon

Within the Enterprise SOC Lab, Sysmon and Wazuh work together to provide detailed endpoint visibility.

The workflow is straightforward:

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

Sysmon generates detailed Windows events, while the Wazuh Agent collects those events and forwards them to the Wazuh platform for processing and analysis.

Together, they provide the foundation for security monitoring within the Enterprise SOC Lab.

---

# Preparing for the Next Chapter

You now have a basic understanding of how the Wazuh platform collects and processes security events.

The next chapter focuses on understanding security logs, where they originate, what information they contain, and why they are essential for security investigations and incident response.

---

# Summary

In this chapter, you learned the purpose of Wazuh and its role within the Enterprise SOC Lab.

Rather than focusing on installation, this chapter explained how Wazuh collects, processes, stores, and displays security events from monitored endpoints.

Understanding these concepts provides a solid foundation for analyzing security events and performing investigations in the upcoming chapters.

---

# Key Takeaways

After completing this chapter, you should understand:

- Wazuh is an open-source security monitoring platform.
- The Wazuh Agent collects security events from monitored endpoints.
- The Wazuh Manager receives and processes collected events.
- The OpenSearch Indexer stores processed security data.
- The Wazuh Dashboard provides a centralized interface for monitoring and investigations.
- Wazuh and Sysmon work together to improve endpoint visibility.

---

# Next Chapter

The next chapter introduces **Understanding Logs**.

Logs are the foundation of every Security Operations Center (SOC). In this chapter, you will learn what logs are, where they originate, how they are collected, and why they play a critical role in security monitoring and incident investigations.

**Topics Covered**

- What Are Logs?
- Types of Security Logs
- Windows Event Logs
- Sysmon Logs
- Wazuh Alerts
- Log Collection Workflow
- Why Logs Matter in a SOC

**Next Document**

`13 - Understanding-Logs.md`

---

# Document Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | July 2026 | Rajeev Kumar | Initial release of the Understanding Wazuh guide. |