# Troubleshooting

---

# Chapter Information

| Property | Details |
|----------|---------|
| **Chapter Number** | 15 |
| **Chapter Title** | Troubleshooting |
| **Difficulty** | Beginner → Advanced |
| **Estimated Reading Time** | 20–30 Minutes |
| **Estimated Lab Time** | Varies |
| **Prerequisite** | Chapters 01–14 |
| **Category** | Troubleshooting Guide |
| **Next Chapter** | Chapter 16 – Lessons Learned |

---

# Document Information

| Property | Details |
|----------|---------|
| **Project** | Enterprise SOC Lab |
| **Document Name** | Troubleshooting |
| **Version** | 1.0 |
| **Author** | Rajeev Kumar |
| **Status** | Published |
| **Last Updated** | July 2026 |

---

# Learning Objectives

After completing this chapter, you will be able to:

- Identify common issues encountered while building the Enterprise SOC Lab.
- Understand the root cause of installation and configuration problems.
- Apply the documented solutions to resolve common errors.
- Verify that services and components are functioning correctly.
- Reduce troubleshooting time during future deployments.

---

# Objective

Building a Security Operations Center (SOC) lab involves installing and configuring multiple operating systems, services, and security tools.

During the development of this Enterprise SOC Lab, several real-world issues were encountered while configuring Ubuntu Server, Wazuh, Windows Agent, Sysmon, and the overall monitoring environment.

This chapter documents those issues along with their root causes, troubleshooting steps, and final resolutions.

Unlike generic troubleshooting guides, the solutions presented here are based on practical issues encountered during the development of this lab.

---

# How to Use This Guide

Each troubleshooting entry follows the same structure to make issues easy to understand and resolve.

Every issue includes:

- Issue Description
- Symptoms
- Root Cause
- Resolution Steps
- Verification
- Lessons Learned

Following this structure helps maintain consistency and allows readers to quickly locate and resolve specific problems.

---

# Troubleshooting Categories

The issues documented in this chapter are grouped into categories based on the affected component.

| Category | Description |
|----------|-------------|
| Ubuntu Server | Operating system installation and configuration issues. |
| VMware | Virtual machine and virtualization-related issues. |
| Wazuh Platform | Wazuh Manager, Dashboard, and Indexer issues. |
| Windows Agent | Agent installation and connectivity problems. |
| Sysmon | Sysmon installation and configuration issues. |
| Event Collection | Log forwarding and event collection problems. |
| Dashboard | Event visibility and indexing issues. |
| Network | Communication issues between virtual machines. |

Organizing issues into categories makes it easier to locate relevant troubleshooting steps.

---

# Troubleshooting Workflow

When troubleshooting an issue, follow a structured approach instead of making random configuration changes.

The recommended workflow is:

```text
Identify the Issue
        │
        ▼
Review Error Messages
        │
        ▼
Determine the Root Cause
        │
        ▼
Apply the Resolution
        │
        ▼
Verify the Fix
        │
        ▼
Document the Outcome
```

Following this workflow helps reduce troubleshooting time and prevents unnecessary configuration changes.

---

# Troubleshooting Best Practices

Keep the following best practices in mind while troubleshooting:

- Change only one configuration at a time.
- Verify each change before proceeding.
- Record the commands used during troubleshooting.
- Restart services only when required.
- Verify service status after making changes.
- Maintain screenshots of important errors whenever possible.
- Document successful resolutions for future reference.

A structured troubleshooting approach improves consistency and makes recurring issues easier to resolve.

---

# Issue 01 – archives.json File Not Generated

## Symptoms

- No events were written to the `archives.json` file.
- Security events were visible in the Wazuh Dashboard, but the archive file remained empty.
- Investigation data was unavailable for later analysis.

## Root Cause

Archive logging was not enabled in the Wazuh Manager configuration.

## Resolution

- Review the Wazuh Manager configuration.
- Enable archive logging if it is disabled.
- Restart the Wazuh Manager service.
- Generate new test events and verify that the `archives.json` file is updated.

## Verification

- Confirm that the `archives.json` file is created.
- Verify that newly generated events are written to the file.

---

# Issue 02 – logall Disabled

## Symptoms

- Security events were not archived.
- Investigation logs were incomplete.
- Historical event data was unavailable.

## Root Cause

The `logall` option was disabled in the Wazuh Manager configuration.

## Resolution

- Enable the `logall` option.
- Restart the Wazuh Manager service.
- Generate new events and verify archive logging.

## Verification

- Confirm that archived logs are generated after enabling `logall`.

---

# Issue 03 – logall_json Disabled

## Symptoms

- JSON archive logs were not generated.
- Some investigation workflows could not be completed.
- Required event data was unavailable in JSON format.

## Root Cause

The `logall_json` option was disabled.

## Resolution

- Enable `logall_json`.
- Restart the Wazuh Manager service.
- Generate new events for testing.

## Verification

- Confirm that JSON archive logs are generated successfully.
- Verify that new events are recorded in the JSON archive.

---

# Issue 04 – Wazuh Agent Disconnected

## Symptoms

- The Windows endpoint appears as **Disconnected** in the Wazuh Dashboard.
- No new events are received from the endpoint.
- The last keepalive timestamp is outdated.

## Root Cause

The Wazuh Agent service is stopped, the manager IP is incorrect, or network connectivity between the endpoint and the Wazuh Manager is unavailable.

## Resolution

- Verify that the Wazuh Agent service is running.
- Confirm that the Manager IP address is correct.
- Verify network connectivity between the endpoint and the Wazuh Manager.
- Restart the Wazuh Agent if necessary.

## Verification

- The agent status changes to **Active** in the Wazuh Dashboard.
- New security events are received successfully.

---

# Issue 05 – Sysmon Events Not Visible in Wazuh

## Symptoms

- Sysmon is installed successfully.
- Sysmon events appear in Event Viewer.
- No Sysmon events appear in the Wazuh Dashboard.

## Root Cause

The Sysmon Operational event channel is not configured in the Wazuh Agent configuration.

## Resolution

- Verify the `ossec.conf` configuration.
- Confirm that the Sysmon Operational event channel is configured correctly.
- Restart the Wazuh Agent.
- Generate new test events.

## Verification

- Sysmon events appear in the Wazuh Dashboard.
- Event details include the Sysmon provider and Event ID.

---

# Issue 06 – Dashboard Does Not Display Recent Events

## Symptoms

- The Wazuh Dashboard loads successfully.
- No recent security events are displayed.
- Event searches return no results.

## Root Cause

Events have not been indexed successfully, or no new events have been generated.

## Resolution

- Generate new endpoint activity.
- Verify that the Wazuh services are running.
- Confirm that events are being forwarded and indexed successfully.

## Verification

- Newly generated events become visible in the dashboard.
- Event searches return expected results.

---

# Issue 07 – Network Communication Problems

## Symptoms

- Virtual machines cannot communicate.
- Agent registration fails.
- Dashboard cannot receive endpoint events.

## Root Cause

Incorrect VMware network configuration or IP addressing.

## Resolution

- Verify the VMware network mode.
- Confirm the IP configuration of each virtual machine.
- Test connectivity between systems.
- Correct any network configuration issues.

## Verification

- Virtual machines communicate successfully.
- Wazuh Agent reconnects to the Manager.
- Security events are forwarded without errors.

---

# Troubleshooting Checklist

Before proceeding to the next chapter, verify the following:

| Status | Verification |
|:------:|--------------|
| ☐ | All virtual machines are running successfully. |
| ☐ | Network connectivity between virtual machines has been verified. |
| ☐ | Wazuh services are running correctly. |
| ☐ | Windows Agent is connected to the Wazuh Manager. |
| ☐ | Sysmon is installed and generating events. |
| ☐ | Sysmon events are visible in the Wazuh Dashboard. |
| ☐ | Archive logging is functioning correctly. |
| ☐ | All identified issues have been resolved successfully. |

---

# Summary

This chapter documented the most common issues encountered while building the Enterprise SOC Lab.

Each issue included its symptoms, root cause, resolution steps, and verification process to provide a structured approach to troubleshooting.

By following these documented solutions, you can resolve common installation, configuration, and event collection issues more efficiently and maintain a stable SOC lab environment.

---

# Key Takeaways

After completing this chapter, you should be able to:

- Identify common issues during Enterprise SOC Lab deployment.
- Determine the root cause of installation and configuration problems.
- Apply structured troubleshooting techniques.
- Verify that services and integrations are functioning correctly.
- Document issues and their resolutions for future reference.

---

# Next Chapter

The next chapter presents the lessons learned while building the Enterprise SOC Lab.

It summarizes practical experience, common mistakes, best practices, and recommendations that can help future learners build and maintain a more stable and efficient SOC lab.

**Topics Covered**

- Lessons Learned During Lab Development
- Common Beginner Mistakes
- Best Practices
- Lab Maintenance Tips
- Recommendations for Future Improvements

**Next Document**

`16 - Lessons-Learned.md`

---

# Document Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | July 2026 | Rajeev Kumar | Initial release of the Troubleshooting guide. |