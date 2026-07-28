# Investigation-18 - Net Config Workstation Process Investigation


# Investigation Information

| Property | Details |
|----------|---------|
| **Investigation ID** | 18 |
| **Investigation Name** | Net Config Workstation Process Investigation |
| **Category** | System Configuration Discovery |
| **Event Category** | Process Creation |
| **Platform** | Windows 10 |
| **Event Source** | Microsoft-Windows-Sysmon |
| **Event ID** | 1 (Process Create) |
| **MITRE ATT&CK** | T1082 – System Information Discovery, T1047 – Windows Management Instrumentation |
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

The objective of this investigation is to analyze the execution of the Windows **net config workstation** command using telemetry collected by Sysmon and Wazuh.

The investigation demonstrates how Sysmon records process creation events and how Wazuh detects Windows system information discovery activity when the command is executed from both Administrator Command Prompt and Administrator Windows PowerShell.

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

The Windows **net config workstation** command was executed on the Windows 10 endpoint from both Administrator Command Prompt and Administrator Windows PowerShell to generate Sysmon Process Create (Event ID 1), allowing the activity to be collected and analyzed in Wazuh.

## Command Prompt

```cmd
net config workstation
```

```text
Computer name                        \\DESKTOP-8GB0J2A
Full Computer name                   DESKTOP-8GB0J2A
User name                            oxhunter24@outlook.com

Workstation active on
        NetBT_Tcpip_{EF22887A-91EF-4633-A2DE-7D088A3DEB47} (000C290BF9E2)

Software version                     Windows 10 Pro

Workstation domain                   WORKGROUP
Logon domain                         MicrosoftAccount

COM Open Timeout (sec)               0
COM Send Count (byte)                16
COM Send Timeout (msec)              250
The command completed successfully.
```

> 📸 **Screenshot Required**
>
> **Title:** Net Config Workstation Command Execution
>
> **Save As:** `screenshots/investigations/net-config-workstation/01-command.png`

---

> **Note**
>
> The execution of the Windows `net config workstation` command generated Wazuh alerts because the activity matched built-in detection rules for Windows system information discovery.
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
| Command (CMD) | `net config workstation` |
| Command (PowerShell) | `"C:\Windows\system32\net.exe" config workstation` |
| Parent Process (CMD) | `cmd.exe` |
| Parent Process (PowerShell) | `powershell.exe` |
| Process Integrity | High (CMD), High (PowerShell) |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Detection Source | `wazuh-alerts` |
| Wazuh Rule ID | 92080 |
| Rule Description | System information discovery activity detected |
| Computer | DESKTOP-8GB0J2A |
| Agent | SOC-Win10 |

> 📸 **Screenshot Required**
>
> **Title:** Net Config Workstation Event in Wazuh Discover
>
> **Save As:** `screenshots/investigations/net-config-workstation/02-wazuh-event.png`

---

# Event Summary

The Windows **net config workstation** command was executed from both **Administrator Command Prompt** and **Administrator Windows PowerShell** on the Windows 10 endpoint.

Sysmon recorded both executions as **Process Create (Event ID 1)** events, and Wazuh generated **Rule 92080 – System information discovery activity detected** because the command collects workstation configuration details that may assist an attacker during the discovery phase.

Although this command is commonly used by system administrators for troubleshooting and system verification, it can also be abused by attackers to collect information about the compromised host before performing additional actions.

---

# Event Analysis

## Command Prompt Execution

| Property | Value |
|----------|-------|
| Parent Process | `cmd.exe` |
| Process | `net.exe` |
| Command | `net config workstation` |
| Process ID (PID) | 5064 |
| Parent Process ID (PPID) | 4976 |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Integrity Level | High |
| Rule ID | 92080 |
| Rule Description | System information discovery activity detected |
| MITRE ATT&CK | T1082 – System Information Discovery |

### Analysis

The command was launched from an elevated Command Prompt, which created a new **net.exe** process. Sysmon captured the complete process creation event, including the executable path, command line, parent process, integrity level, process identifiers, and user context.

Wazuh correlated the event with Rule **92080**, identifying the activity as **System Information Discovery** because the command retrieves workstation configuration information that may help an attacker understand the target environment.

> 📸 **Screenshot Required**
>
> **Title:** Command Prompt Process Create Event
>
> **Save As:** `screenshots/investigations/net-config-workstation/03-cmd-event.png`

---

## Windows PowerShell Execution

| Property | Value |
|----------|-------|
| Parent Process | `powershell.exe` |
| Process | `net.exe` |
| Command | `"C:\Windows\System32\net.exe" config workstation` |
| Process ID (PID) | 9004 |
| Parent Process ID (PPID) | 8484 |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Integrity Level | High |
| Rule ID | 92080 |
| Rule Description | System information discovery activity detected |
| MITRE ATT&CK | T1047 – Windows Management Instrumentation (Wazuh Mapping) |

### Analysis

The same command was executed from Windows PowerShell, which spawned **net.exe** as a child process.

Sysmon recorded the execution with full parent-child process relationships, while Wazuh generated the same Rule **92080**. The alert maps the PowerShell execution to **MITRE ATT&CK T1047** according to the built-in Wazuh detection logic.

No evidence of malicious behavior was observed during this investigation. The command was executed manually within the Enterprise SOC Lab to generate telemetry for analyst training and investigation.

> 📸 **Screenshot Required**
>
> **Title:** Windows PowerShell Process Create Event
>
> **Save As:** `screenshots/investigations/net-config-workstation/04-powershell-event.png`

---

## Analyst Observation

The **net config workstation** command is a legitimate Windows administrative utility that displays workstation configuration details such as the computer name, logged-on user, Windows version, and workstation service settings.

From a defensive perspective, isolated execution of this command is generally considered low risk. However, repeated execution alongside other discovery commands (such as `systeminfo`, `hostname`, `ipconfig`, `whoami`, or `net user`) may indicate reconnaissance activity performed by an attacker after gaining initial access.

SOC analysts should always evaluate the surrounding process activity, parent process, user context, and timeline before determining whether the execution is benign or suspicious.

---

# Timeline

| Time (UTC) | Event |
|------------|-------|
| 2026-07-28 05:33:30.001 | `net config workstation` executed from Windows PowerShell. |
| 2026-07-28 05:33:31.143 | Wazuh generated Rule **92080** for the PowerShell execution. |
| 2026-07-28 05:33:46.173 | `net config workstation` executed from Administrator Command Prompt. |
| 2026-07-28 05:33:47.311 | Wazuh generated Rule **92080** for the Command Prompt execution. |

---

# 5W Analysis

| Question | Answer |
|----------|--------|
| **Who** | User **DESKTOP-8GB0J2A\Oxhun** executed the command. |
| **What** | The Windows **net config workstation** command was executed to display workstation configuration information. |
| **When** | 2026-07-28 between **05:33:30 UTC** and **05:33:46 UTC**. |
| **Where** | Windows 10 endpoint **SOC-Win10** monitored by Wazuh and Sysmon. |
| **Why** | To generate Sysmon telemetry for Enterprise SOC Lab investigation and demonstrate how Wazuh detects Windows system information discovery activity. |

---

# MITRE ATT&CK Mapping

| Technique ID | Technique | Tactic |
|--------------|-----------|--------|
| T1082 | System Information Discovery | Discovery |
| T1047 | Windows Management Instrumentation *(Wazuh Detection Mapping)* | Execution |

---

# Findings

- The Windows **net.exe** utility was executed successfully from both Command Prompt and Windows PowerShell.
- Sysmon generated **Event ID 1 (Process Create)** for both executions.
- Wazuh generated **Rule 92080 – System information discovery activity detected**.
- The command was executed with **High Integrity** privileges.
- Parent-child process relationships were successfully captured.
- No persistence, privilege escalation, or lateral movement activity was observed.
- The activity was performed intentionally inside the Enterprise SOC Lab for analyst training.

---

# Final Verdict

**Status:** Benign Administrative Activity (Lab Generated)

The execution of **net config workstation** represents legitimate administrative behavior. Although the command is commonly used for troubleshooting and system administration, attackers may also use it during the reconnaissance phase to collect workstation configuration details.

In this investigation, the execution was intentionally performed in a controlled lab environment to generate Sysmon and Wazuh telemetry for SOC analyst training. No indicators of malicious activity were identified.

---

# Analyst Notes

SOC analysts should review the execution context whenever **net config workstation** is observed.

The command becomes more significant when executed together with multiple discovery commands such as:

- `whoami`
- `hostname`
- `systeminfo`
- `ipconfig`
- `net user`
- `net localgroup`
- `net view`

When these commands are executed within a short period by the same user or process tree, they may indicate attacker reconnaissance following initial access.

---

# Lab Validation

| Validation Item | Status |
|-----------------|--------|
| Command Executed Successfully | Yes |
| Sysmon Event Generated | Yes |
| Wazuh Alert Generated | Yes |
| Rule ID Verified | 92080 |
| MITRE Mapping Verified | T1082, T1047 |
| Investigation Completed | Yes |

---

# Conclusion

This investigation demonstrated how Windows **net config workstation** activity is recorded by Sysmon and detected by Wazuh.

Analysts learned how to identify the command, examine process creation events, verify parent-child process relationships, and correlate the activity with Wazuh Rule **92080** and its associated MITRE ATT&CK mappings.

Although the activity was benign in this lab, similar discovery commands should always be evaluated within the broader investigation timeline to determine whether they are part of normal administration or an adversary's reconnaissance.

---

# Next Investigation

**19-Investigation-Net-Config-Server.md**

---

# Revision History

| Version | Date | Author | Changes |
|----------|------|--------|---------|
| 1.0 | 2026-07-28 | Rajeev | Initial investigation created for Windows `net config workstation` command using Sysmon and Wazuh telemetry. |