# Investigation-19 - Net Config Server Process Investigation

---

# Investigation Information

| Property | Details |
|----------|---------|
| **Investigation ID** | 19 |
| **Investigation Name** | Net Config Server Process Investigation |
| **Category** | System Configuration Discovery |
| **Event Category** | Process Creation |
| **Platform** | Windows 10 |
| **Event Source** | Microsoft-Windows-Sysmon |
| **Event ID** | 1 (Process Create) |
| **MITRE ATT&CK** | T1087 – Account Discovery, T1059.001 – PowerShell |
| **Difficulty** | Beginner |

---

# Related References

The following reference documents provide additional information related to this investigation.

| Document | Purpose |
|----------|---------|
| `commands/Windows-Command-Reference.md` | Windows command reference used throughout the Enterprise SOC Lab investigations. |
| `commands/Wazuh-Search-Queries.md` | Wazuh Threat Hunting search queries used to locate investigation events. |
| `commands/MITRE-Reference.md` | MITRE ATT&CK techniques associated with Windows command-line activities used throughout this lab. |

---

# Objective

The objective of this investigation is to analyze the execution of the Windows **net config server** command using telemetry collected by Sysmon and Wazuh.

The investigation demonstrates how Sysmon records process creation events and how Wazuh detects discovery activity when the command is executed from both Administrator Command Prompt and Administrator Windows PowerShell.

---

# Lab Environment

| Component | Value |
|----------|-------|
| SIEM | Wazuh |
| Endpoint | Windows 10 |
| Agent Name | SOC-Win10 |
| Agent ID | 001 |
| Agent IP | 192.168.92.134 |
| Manager | wazuh-server |
| Log Source | Microsoft-Windows-Sysmon |

---

# Lab Execution

The Windows **net config server** command was executed on the Windows 10 endpoint from both Administrator Command Prompt and Administrator Windows PowerShell to generate Sysmon Process Create (Event ID 1), allowing the activity to be collected and analyzed in Wazuh.

## Command Prompt

```cmd
net config server
```

```text
Server Name                           \\DESKTOP-8GB0J2A
Server Comment

Software version                      Windows 10 Pro
Server is active on
        NetbiosSmb (DESKTOP-8GB0J2A)
        NetBT_Tcpip_{EF22887A-91EF-4633-A2DE-7D088A3DEB47} (DESKTOP-8GB0J2A)


Server hidden                         No
Maximum Logged On Users               20
Maximum open files per session        16384

Idle session time (min)               15
The command completed successfully.
```

> 📸 **Screenshot Required**
>
> **Title:** Net Config Server Command Execution
>
> **Save As:** `screenshots/investigations/net-config-server/01-command.png`

---

> **Note**
>
> The execution of the Windows `net config server` command generated Wazuh alerts because the activity matched built-in discovery detection rules.
>
> The process creation events were recorded by Sysmon (Event ID 1) and indexed in both the `wazuh-alerts` and `wazuh-archives` indices.

---

# Event Information

| Property | Value |
|----------|-------|
| Event Source | Microsoft-Windows-Sysmon |
| Event ID | 1 (Process Create) |
| Executable | `C:\Windows\System32\net.exe` |
| Original File Name | `net.exe` |
| Description | Net Command |
| Company | Microsoft Corporation |
| Command (CMD) | `net config server` |
| Command (PowerShell) | `"C:\Windows\system32\net.exe" config server` |
| Parent Process (CMD) | `cmd.exe` |
| Parent Process (PowerShell) | `powershell.exe` |
| Process Integrity | High (CMD), High (PowerShell) |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Detection Source | `wazuh-alerts` |
| Wazuh Rule ID (CMD) | 92031 |
| Rule Description (CMD) | Discovery activity executed |
| Wazuh Rule ID (PowerShell) | 92033 |
| Rule Description (PowerShell) | Discovery activity spawned via powershell execution |
| Computer | DESKTOP-8GB0J2A |
| Agent | SOC-Win10 |

> 📸 **Screenshot Required**
>
> **Title:** Net Config Server Event in Wazuh Discover
>
> **Save As:** `screenshots/investigations/net-config-server/02-wazuh-event.png`

---

# Event Summary

The Windows **net config server** command was executed from both **Administrator Command Prompt** and **Administrator Windows PowerShell** on the Windows 10 endpoint.

Sysmon recorded both executions as **Process Create (Event ID 1)** events. Wazuh generated **Rule 92031 – Discovery activity executed** for the Command Prompt execution and **Rule 92033 – Discovery activity spawned via powershell execution** for the PowerShell execution.

The **net config server** command displays configuration information for the Windows Server service. While it is commonly used by administrators for system management and troubleshooting, attackers may also execute it during the discovery phase to collect information about the target system.

---

# Event Analysis

## Command Prompt Execution

| Property | Value |
|----------|-------|
| Parent Process | `cmd.exe` |
| Process | `net.exe` |
| Command | `net config server` |
| Process ID (PID) | 3664 |
| Parent Process ID (PPID) | 4976 |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Integrity Level | High |
| Rule ID | 92031 |
| Rule Description | Discovery activity executed |
| MITRE ATT&CK | T1087 – Account Discovery |

### Analysis

The **net config server** command was executed from an elevated Command Prompt, creating a new **net.exe** process.

Sysmon successfully recorded the process creation event, including the executable path, command line, process identifiers, integrity level, parent process, and user context.

Wazuh correlated the activity with **Rule 92031**, identifying it as a discovery-related command based on the built-in detection logic.

> 📸 **Screenshot Required**
>
> **Title:** Command Prompt Process Create Event
>
> **Save As:** `screenshots/investigations/net-config-server/03-cmd-event.png

---

## Windows PowerShell Execution

| Property | Value |
|----------|-------|
| Parent Process | `powershell.exe` |
| Process | `net.exe` |
| Command | `"C:\Windows\System32\net.exe" config server` |
| Process ID (PID) | 2328 |
| Parent Process ID (PPID) | 8484 |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Integrity Level | High |
| Rule ID | 92033 |
| Rule Description | Discovery activity spawned via powershell execution |
| MITRE ATT&CK | T1087 – Account Discovery, T1059.001 – PowerShell |

### Analysis

The same command was executed from Windows PowerShell, which spawned **net.exe** as a child process.

Sysmon captured the parent-child process relationship, while Wazuh generated **Rule 92033**, indicating that the discovery activity originated from PowerShell. The alert maps the activity to **MITRE ATT&CK T1087 (Account Discovery)** and **T1059.001 (PowerShell)** according to the Wazuh detection rules.

No malicious activity was identified during this investigation. The command was executed intentionally within the Enterprise SOC Lab to generate telemetry for SOC analyst training.

> 📸 **Screenshot Required**
>
> **Title:** Windows PowerShell Process Create Event
>
> **Save As:** `screenshots/investigations/net-config-server/04-powershell-event.png`

---

## Analyst Observation

The **net config server** command is a legitimate Windows administrative utility used to display the current configuration of the Server service.

Although the command is benign when executed by administrators, it can also be used by attackers during the discovery phase to understand server-related configuration before attempting lateral movement or additional reconnaissance.

SOC analysts should evaluate the execution context, parent process, user account, integrity level, and surrounding process activity before determining whether the execution represents normal administrative activity or suspicious reconnaissance.

---

# Timeline

| Time (UTC) | Event |
|------------|-------|
| 2026-07-28 05:55:07.659 | `net config server` executed from Administrator Command Prompt. |
| 2026-07-28 05:55:08.807 | Wazuh generated Rule **92031** for the Command Prompt execution. |
| 2026-07-28 05:55:17.079 | `net config server` executed from Windows PowerShell. |
| 2026-07-28 05:55:18.213 | Wazuh generated Rule **92033** for the PowerShell execution. |

---

# 5W Analysis

| Question | Answer |
|----------|--------|
| **Who** | User **DESKTOP-8GB0J2A\Oxhun** executed the command. |
| **What** | The Windows **net config server** command was executed to display Server service configuration information. |
| **When** | 2026-07-28 between **05:55:07 UTC** and **05:55:17 UTC**. |
| **Where** | Windows 10 endpoint **SOC-Win10** monitored by Sysmon and Wazuh. |
| **Why** | To generate Sysmon telemetry for Enterprise SOC Lab investigation and demonstrate how Wazuh detects Windows discovery activity. |

---

# MITRE ATT&CK Mapping

| Technique ID | Technique | Tactic |
|--------------|-----------|--------|
| T1087 | Account Discovery | Discovery |
| T1059.001 | PowerShell | Execution |

---

# Findings

- The Windows **net.exe** utility was executed successfully from both Command Prompt and Windows PowerShell.
- Sysmon generated **Event ID 1 (Process Create)** for both executions.
- Wazuh generated **Rule 92031** for the Command Prompt execution.
- Wazuh generated **Rule 92033** for the Windows PowerShell execution.
- Both executions were performed with **High Integrity** privileges.
- Parent-child process relationships were successfully recorded by Sysmon.
- No evidence of privilege escalation, persistence, credential access, or lateral movement was observed.
- The activity was intentionally generated within the Enterprise SOC Lab for analyst training.

---

# Final Verdict

**Status:** Benign Administrative Activity (Lab Generated)

The execution of **net config server** represents legitimate Windows administrative activity used to display Server service configuration.

Although the command is commonly used by administrators, similar discovery commands may also be executed by attackers during the reconnaissance phase after obtaining access to a system.

In this investigation, the execution was intentionally performed inside the Enterprise SOC Lab to generate Sysmon telemetry and validate Wazuh detections. No malicious activity was identified.

---

# Analyst Notes

SOC analysts should investigate the surrounding context whenever **net config server** is observed.

While a single execution is usually benign, repeated execution alongside other discovery commands such as:

- `whoami`
- `hostname`
- `systeminfo`
- `ipconfig`
- `net user`
- `net localgroup`
- `net accounts`
- `net config workstation`

may indicate reconnaissance activity performed before additional attacker actions.

Analysts should always correlate process execution with the parent process, user account, timeline, and other related events before reaching a conclusion.

---

# Lab Validation

| Validation Item | Status |
|-----------------|--------|
| Command Executed Successfully | Yes |
| Sysmon Event Generated | Yes |
| Wazuh Alert Generated | Yes |
| Rule ID (CMD) Verified | 92031 |
| Rule ID (PowerShell) Verified | 92033 |
| MITRE Mapping Verified | T1087, T1059.001 |
| Investigation Completed | Yes |

---

# Conclusion

This investigation demonstrated how Windows **net config server** activity is captured by Sysmon and detected by Wazuh.

Analysts learned how to identify the command, validate the parent-child process relationship, review process creation events, and correlate the activity with Wazuh Rules **92031** and **92033** along with their associated MITRE ATT&CK mappings.

Although the activity was benign in this lab, SOC analysts should always evaluate similar discovery commands within the complete attack timeline to distinguish legitimate administrative actions from potential attacker reconnaissance.

---

# Next Investigation

**20-Windows-Command-Investigation-Summary.md**

---

# Revision History

| Version | Date | Author | Changes |
|----------|------|--------|---------|
| 1.0 | 2026-07-28 | Rajeev | Initial investigation created for Windows `net config server` command using Sysmon and Wazuh telemetry. |