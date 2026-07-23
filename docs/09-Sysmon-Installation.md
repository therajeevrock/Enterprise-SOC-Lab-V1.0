# Sysmon Installation

---

# Chapter Information

| Property                   | Details                              |
|----------------------------|--------------------------------------|
| **Chapter Number**         | 09                                   |
| **Chapter Title**          | Sysmon Installation                  |
| **Difficulty**             | Intermediate                         |
| **Estimated Reading Time** | 15–20 Minutes                        |
| **Estimated Lab Time**     | 20–30 Minutes                        |
| **Prerequisite**           | Chapter 08 – Windows Agent Deployment|
| **Category**               | Installation Guide                   |
| **Related Troubleshooting**| Troubleshooting.md                   |
| **Next Chapter**           | Chapter 10 – Wazuh–Sysmon Integration|

---

# Document Information

| Property          | Details             |
|-------------------|---------------------|
| **Project**       | Enterprise SOC Lab  |
| **Document Name** | Sysmon Installation |
| **Version**       | 1.0                 |
| **Author**        | Rajeev Rock         |
| **Status**        | Published           |
| **Last Updated**  | July 2026           |

---

# Learning Objectives

After completing this chapter, you will be able to:

- Understand the purpose of Sysmon.
- Download Sysmon from the official Microsoft Sysinternals website.
- Install Sysmon on the Windows 10 endpoint.
- Apply a Sysmon configuration file.
- Verify that Sysmon is installed and running successfully.
- Prepare the endpoint for advanced event collection in Wazuh.

---

# Objective

The objective of this chapter is to install Sysmon (System Monitor) on the Windows 10 endpoint to enhance Windows event logging.

By default, Windows generates a limited set of security logs. Sysmon extends this capability by recording detailed system activity, including process creation, network connections, file creation, driver loading, registry modifications, and other security-relevant events.

Once installed, Sysmon generates high-quality telemetry that will later be collected by the Wazuh Agent and forwarded to the Wazuh platform for monitoring and security investigations.

This chapter focuses only on installing, configuring, and verifying Sysmon. Event analysis, detection rules, and SOC investigations will be covered in the following chapters.

---

# What is Sysmon?

Sysmon (System Monitor) is a Windows system service developed by Microsoft as part of the Sysinternals Suite.

It continuously monitors system activity and records detailed events in the **Microsoft-Windows-Sysmon/Operational** event log.

Unlike the default Windows logs, Sysmon provides much deeper visibility into endpoint activity, making it an essential tool for security monitoring, digital forensics, incident response, and threat hunting.

Within the Enterprise SOC Lab, Sysmon serves as the primary source of Windows endpoint telemetry that will later be analyzed through the Wazuh platform.

---

# Why Install Sysmon?

Without Sysmon, Windows records only a limited amount of security information.

Installing Sysmon provides significantly greater visibility into endpoint activity by generating detailed security events.

Key benefits include:

- Detailed process creation logging.
- Network connection monitoring.
- Registry activity monitoring.
- Driver and DLL loading visibility.
- File creation and modification events.
- Process injection detection support.
- Improved endpoint visibility for threat hunting.
- Rich telemetry for Wazuh security monitoring.

Sysmon acts as the primary telemetry source that enables the Enterprise SOC Lab to perform realistic SOC investigations.

---

# Installation Prerequisites

Before installing Sysmon, verify that the following requirements have been completed.

| Requirement                                     | Status |
|------------------------------------------------ |--------|
| Windows 10 endpoint installed                   |   ✓   |
| Wazuh Agent installed                           |   ✓   |
| Wazuh Agent connected to the Wazuh Manager      |   ✓   |
| Windows endpoint visible in the Wazuh Dashboard |   ✓   |
| Administrator privileges available              |   ✓   |
| Internet connectivity available                 |   ✓   |

Completing these prerequisites ensures that Sysmon can be installed successfully and that its events can later be collected by the Wazuh platform.

---

# Download Sysmon

Download Sysmon from the official Microsoft Sysinternals website.

Sysmon is distributed as part of the Sysinternals Suite and is maintained by Microsoft.

Using the official release ensures that the software is authentic, secure, and compatible with Windows.

## Official Download Source

Download Sysmon only from the official Microsoft Sysinternals website.

| Resource | Link |
|----------|------|
| Sysmon (System Monitor) | https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon |

The latest version of Sysmon is available from the official Microsoft Sysinternals download page maintained by Microsoft.

> **Note**
>
> Always download Sysmon from the official Microsoft Sysinternals website to ensure authenticity, integrity, and compatibility with your Windows operating system.

> **📸 Screenshot Required**
>
> **Title:** Sysmon Official Download Page
>
> **Save As:** `screenshots/sysmon/01-sysmon-download.png`

---

# Download the Sysmon Configuration File

Sysmon requires a configuration file to determine which Windows events should be monitored.

Without a configuration file, Sysmon records only a limited number of events.

For this Enterprise SOC Lab, use the Sysmon configuration file included with this project.

## Sysmon Configuration Source

| Resource | Link |
|----------|------|
| Sysmon Configuration (SwiftOnSecurity) | https://github.com/SwiftOnSecurity/sysmon-config |

Store both the Sysmon executable and the configuration file in a dedicated directory.

Example:

```text
C:\SOC\Sysmon\
        ├── Sysmon64.exe
        └── sysmonconfig-export.xml
```

> **📸 Screenshot Required**
>
> **Title:** Sysmon Configuration Repository
>
> **Save As:** `screenshots/sysmon/02-sysmon-config-download.png`
---

# Extract the Sysmon Package

After downloading the Sysmon ZIP package, extract its contents to the following directory:

```text
C:\SOC\Sysmon\
```

The extracted folder should contain:

```text
Eula.txt
Sysmon.exe
Sysmon64.exe
Sysmon64a.exe
sysmonconfig-export.xml
```

> **📸 Screenshot Required**
>
> **Title:** Extracted Sysmon Files
>
> **Save As:** `screenshots/sysmon/03-sysmon-extracted-files.png`

---

# Install Sysmon

Open **Command Prompt** as **Administrator**.

Navigate to the directory containing **Sysmon64.exe** and the configuration file.

Run the following command:

```cmd
Sysmon64.exe -accepteula -i sysmonconfig-export.xml
```

The command performs the following actions:
- Accepts the Sysmon license agreement.
- Installs the Sysmon service.
- Applies the Sysmon configuration file.
- Starts monitoring system activity immediately.

If the installation is successful, a confirmation message will be displayed.

> **📸 Screenshot Required**
>
> **Title:** Sysmon Installation
>
> **Save As:** `screenshots/sysmon/04-sysmon-installation.png`

---

# Verify the Installation

After the installation completes successfully, verify that the Sysmon service has been installed.

Run the following command:

```cmd
Sysmon64.exe -c
```

The command displays the active Sysmon configuration currently loaded on the system.

A successful response confirms that Sysmon has been installed correctly and that the configuration file has been applied.

> **📸 Screenshot Required**
>
> **Title:** Sysmon Configuration Verification
>
> **Save As:** `screenshots/sysmon/05-sysmon-configuration.png`

---

# Verify the Sysmon Service

After installing Sysmon, verify that the Sysmon service is running correctly.

Open **Command Prompt** as **Administrator** and run the following command:

```cmd
sc query Sysmon64
```

A successful installation should display:

```text
STATE : RUNNING
```

If the service is not running, review the installation process before proceeding.

> **📸 Screenshot Required**
>
> **Title:** Sysmon Service Status
>
> **Save As:** `screenshots/sysmon/06-sysmon-service.png`

---

# Verify the Sysmon Event Log

Open **Event Viewer** and navigate to:

```text
Applications and Services Logs
        ↓
Microsoft
        ↓
Windows
        ↓
Sysmon
        ↓
Operational
```

If Sysmon has been installed successfully, the **Operational** log should contain recorded events.

> **📸 Screenshot Required**
>
> **Title:** Sysmon Operational Log
>
> **Save As:** `screenshots/sysmon/07-event-viewer.png`

---

# Generate a Test Event

To verify that Sysmon is actively monitoring the endpoint, generate a simple process creation event.

Open **Command Prompt** and run:

```cmd
notepad.exe
```

After launching Notepad, return to the **Sysmon Operational** log and confirm that a new event has been recorded.

This confirms that Sysmon is actively monitoring system activity.

> **📸 Screenshot Required**
>
> **Title:** Sysmon Test Event
>
> **Save As:** `screenshots/sysmon/08-test-event.png`

---

# Installation Verification

Before proceeding to the next chapter, verify the following:

| Verification | Expected Result |
|--------------|-----------------|
| Sysmon installed successfully | ✓ |
| Sysmon service is running | ✓ |
| Sysmon Operational log is available | ✓ |
| Test event generated successfully | ✓ |

If all verification steps are successful, the Windows endpoint is ready for integration with the Wazuh platform.

> **📸 Screenshot Required**
>
> **Title:** Sysmon Installation Verified
>
> **Save As:** `screenshots/sysmon/09-installation-verified.png`

---

# Installation Checklist

Before proceeding to the next chapter, verify that Sysmon has been installed and configured successfully.

| Status | Task |
|:------:|------|
| ☐ | Sysmon downloaded successfully |
| ☐ | Sysmon package extracted |
| ☐ | Sysmon configuration file is available |
| ☐ | Sysmon installed successfully |
| ☐ | Sysmon service is running |
| ☐ | Sysmon Operational log is available |
| ☐ | Test event generated successfully |
| ☐ | Endpoint is ready for Wazuh integration |

---

# Common Installation Issues

### 1. Sysmon service is not running

**Possible Cause**

- Installation failed.
- Sysmon was not installed with administrator privileges.

**Resolution**

- Open Command Prompt as Administrator.
- Reinstall Sysmon using the correct installation command.
- Verify the service status.

---

### 2. Sysmon Operational log is missing

**Possible Cause**

- Sysmon installation was unsuccessful.
- The Event Viewer has not refreshed.

**Resolution**

- Verify the Sysmon service.
- Restart the Windows endpoint if necessary.
- Open Event Viewer again and check the Operational log.

---

### 3. Configuration file cannot be loaded

**Possible Cause**

- Incorrect configuration file name.
- Invalid file path.
- XML configuration file is corrupted.

**Resolution**

- Verify that `sysmonconfig-export.xml` exists.
- Confirm that the installation command references the correct file.
- Download the configuration file again if necessary.

---

### 4. No Sysmon events are generated

**Possible Cause**

- Sysmon is not running.
- No system activity has occurred yet.

**Resolution**

- Confirm that the Sysmon service is running.
- Generate a simple test event (for example, launch Notepad).
- Refresh the Sysmon Operational log.

> **Need Help?**
>
> Refer to **Troubleshooting.md** for detailed solutions, investigation steps, and real-world issues encountered while building this Enterprise SOC Lab.

---

# Summary

In this chapter, Sysmon was successfully installed on the Windows 10 endpoint using the official Microsoft Sysinternals release.

A Sysmon configuration file was applied during installation to enable detailed endpoint monitoring. The installation was verified by checking the Sysmon service, reviewing the Operational event log, and generating a test event.

The Windows endpoint is now capable of producing rich security telemetry that can be collected and analyzed by the Wazuh platform.

---

# Key Takeaways

After completing this chapter, you can:

- Download Sysmon from the official Microsoft Sysinternals website.
- Install Sysmon on a Windows endpoint.
- Apply a Sysmon configuration file.
- Verify the Sysmon service.
- Confirm that Sysmon events are being generated.
- Prepare the endpoint for integration with Wazuh.

---

# Next Chapter

The next chapter focuses on integrating Sysmon with the Wazuh platform.

You will configure the Wazuh Agent to collect Sysmon events and verify that the generated telemetry is successfully forwarded to the Wazuh Manager and displayed in the Wazuh Dashboard.

**Topics Covered**

- Configuring the Wazuh Agent for Sysmon
- Collecting Sysmon Operational logs
- Verifying event forwarding
- Confirming log visibility in the Wazuh Dashboard
- Preparing the environment for SOC investigations

**Next Document**

`10 - Wazuh-Sysmon-Integration.md`

---

# Document Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | July 2026 | Rajeev Rock | Initial release of the Sysmon Installation guide. |