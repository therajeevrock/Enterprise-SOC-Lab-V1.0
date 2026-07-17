# Investigation 02 - Hostname

---

# Investigation Information

| Property | Details |
|----------|---------|
| **Investigation ID** | 02 |
| **Investigation Name** | Hostname Process Investigation |
| **Category** | Host Enumeration |
| **Event Category** | Process Creation |
| **Platform** | Windows 10 |
| **Event Source** | Microsoft-Windows-Sysmon |
| **Event ID** | 1 (Process Create) |
| **MITRE ATTACK** | T1082 - System-Information-Discovery |
| **Difficulty** | Beginner |

---

# Related References

The following reference documents provide additional information related to this investigation.

| Document | Purpose |
|----------|---------|
| `commands/Windows-Command-Reference.md` | Windows command reference used throughout the Enterprise SOC Lab investigations. |
| `commands/Wazuh-Search-Queries.md` | Wazuh Threat Hunting search queries used to locate investigation events. |

---


# Objective

The objective of this investigation is to analyze the execution of the Windows **hostname** command using the telemetry collected by Sysmon and Wazuh.

The investigation focuses on verifying the process execution, identifying the executing user and parent process, and determining whether the activity is legitimate or requires further investigation.