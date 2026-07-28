# Investigation-17 - Net Accounts Process Investigation


# Investigation Information

| Property | Details |
|----------|---------|
| **Investigation ID** | 17 |
| **Investigation Name** | Net Accounts Process Investigation |
| **Category** | Account Policy Discovery |
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

The objective of this investigation is to analyze the execution of the Windows **net accounts** command using telemetry collected by Sysmon and Wazuh.

The investigation demonstrates how Sysmon records process creation events and how Wazuh detects Windows discovery activity when the command is executed from both Administrator Command Prompt and Administrator Windows PowerShell.

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

The Windows **net accounts** command was executed on the Windows 10 endpoint from both Administrator Command Prompt and Administrator Windows PowerShell to generate Sysmon Process Create (Event ID 1), allowing the activity to be collected and analyzed in Wazuh.

## Command Prompt

```cmd
net accounts
```

```text
Force user logoff how long after time expires?:       Never
Minimum password age (days):                          0
Maximum password age (days):                          42
Minimum password length:                              0
Length of password history maintained:                None
Lockout threshold:                                    10
Lockout duration (minutes):                           10
Lockout observation window (minutes):                 10
Computer role:                                        WORKSTATION
The command completed successfully.


```

> 📸 **Screenshot Required**
>
> **Title:** Net Accounts Command Execution
>
> **Save As:** `screenshots/investigations/net-accounts/01-command.png`

---

> **Note**
>
> The execution of the Windows `net accounts` command generated Wazuh alerts because the activity matched built-in detection rules for Windows discovery activity.
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
| Command (CMD) | `net account` *(Recorded in telemetry)* |
| Command (PowerShell) | `"C:\Windows\system32\net.exe" account` *(Recorded in telemetry)* |
| Expected Windows Command | `net accounts` |
| Parent Process (CMD) | `cmd.exe` |
| Parent Process (PowerShell) | `powershell.exe` |
| Process Integrity | High (CMD), High (PowerShell) |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Detection Source | `wazuh-alerts` |
| Wazuh Rule IDs | 92031 (CMD), 92033 (PowerShell) |
| Computer | DESKTOP-8GB0J2A |
| Agent | SOC-Win10 |

> 📸 **Screenshot Required**
>
> **Title:** Net Accounts Event in Wazuh Discover
>
> **Save As:** `screenshots/investigations/net-accounts/02-wazuh-event.png`

---

# Event Summary

The Windows **net accounts** command was executed from both Administrator Command Prompt and Administrator Windows PowerShell on the Windows 10 endpoint.

Sysmon successfully generated **Event ID 1 (Process Create)** for both executions, and Wazuh detected the activity using its built-in Windows discovery rules.

The Command Prompt execution triggered **Rule 92031 (Discovery activity executed)**, while the Windows PowerShell execution triggered **Rule 92033 (Discovery activity spawned via powershell execution)**.

Both events involved the legitimate Microsoft **net.exe** binary executing with **High Integrity** privileges from the trusted **C:\Windows\System32** directory.

---

# Event Analysis

## Command Prompt Execution

| Property | Value |
|----------|-------|
| Command | `net account` *(Recorded in telemetry)* |
| Process | `C:\Windows\System32\net.exe` |
| Parent Process | `cmd.exe` |
| Process ID (PID) | 272 |
| Parent Process ID (PPID) | 4976 |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Integrity Level | High |
| Event ID | 1 |
| Wazuh Rule | 92031 |
| Rule Description | Discovery activity executed |
| MITRE ATT&CK | T1087 – Account Discovery |
| Execution Time (UTC) | 2026-07-28 05:04:48.426 |

> 📸 **Screenshot Required**
>
> **Title:** Command Prompt Process Create Event
>
> **Save As:** `screenshots/investigations/net-accounts/03-cmd-event.png`

---

## Windows PowerShell Execution

| Property | Value |
|----------|-------|
| Command | `"C:\Windows\system32\net.exe" account` *(Recorded in telemetry)* |
| Process | `C:\Windows\System32\net.exe` |
| Parent Process | `powershell.exe` |
| Process ID (PID) | 7084 |
| Parent Process ID (PPID) | 8484 |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Integrity Level | High |
| Event ID | 1 |
| Wazuh Rule | 92033 |
| Rule Description | Discovery activity spawned via powershell execution |
| MITRE ATT&CK | T1087 – Account Discovery, T1059.001 – PowerShell |
| Execution Time (UTC) | 2026-07-28 05:04:06.198 |

> 📸 **Screenshot Required**
>
> **Title:** Windows PowerShell Process Create Event
>
> **Save As:** `screenshots/investigations/net-accounts/04-powershell-event.png`

---

# Timeline

| Time (UTC) | Activity |
|------------|----------|
| 2026-07-28 05:04:06 | Administrator Windows PowerShell executed `"C:\Windows\System32\net.exe" account` |
| 2026-07-28 05:04:06 | Sysmon generated Event ID 1 |
| 2026-07-28 05:04:07 | Wazuh generated Rule 92033 (Discovery activity spawned via PowerShell execution) |
| 2026-07-28 05:04:48 | Administrator Command Prompt executed `net account` |
| 2026-07-28 05:04:48 | Sysmon generated Event ID 1 |
| 2026-07-28 05:04:49 | Wazuh generated Rule 92031 (Discovery activity executed) |

---

# 5W Analysis

| Question | Analysis |
|----------|----------|
| **Who** | `DESKTOP-8GB0J2A\Oxhun` |
| **What** | Executed the Windows `net account` command to enumerate local account policy information. |
| **When** | 28 July 2026 |
| **Where** | Windows 10 endpoint monitored by Sysmon and Wazuh |
| **Why** | Validate Sysmon telemetry collection and Wazuh detection of Windows account discovery activity executed through Command Prompt and Windows PowerShell. |

---

# MITRE ATT&CK Mapping

| Tactic | Technique ID | Technique |
|---------|--------------|-----------|
| Discovery | T1087 | Account Discovery |
| Execution | T1059.001 | PowerShell |

---

# Investigation Findings

- Legitimate Microsoft `net.exe` executed successfully.
- Executed from the trusted `C:\Windows\System32` directory.
- Executed from both Administrator Command Prompt and Administrator Windows PowerShell.
- Both executions generated Sysmon Event ID 1.
- Wazuh generated Rule **92031** for the Command Prompt execution.
- Wazuh generated Rule **92033** for the Windows PowerShell execution.
- Both executions ran with High Integrity privileges.
- No suspicious execution path or malicious command-line arguments were identified.
- The observed activity is consistent with legitimate administrative account policy enumeration.

---

# Final Verdict

| Property | Result |
|----------|--------|
| Classification | Benign Administrative Activity |
| Severity | Low |
| Risk | Low |
| Detection Status | Alert Generated |
| Escalation Required | No |
| Investigation Status | Closed |

---

# Analyst Notes

The Windows `net accounts` command is a legitimate Microsoft administrative utility used to display or configure password and logon policy settings for the local computer or domain. Administrators commonly use this command to review password age, password history, lockout thresholds, and related account security policies.

Because the command reveals security policy information, attackers may also execute it during the discovery phase to understand account restrictions before attempting password attacks or privilege escalation. As a result, security monitoring platforms may classify this activity as Windows account discovery.

In this investigation, the command was executed from the trusted Microsoft `net.exe` binary with High Integrity privileges. Sysmon successfully recorded both Process Create events, and Wazuh generated Rules **92031** and **92033**, mapping the activity to **T1087 (Account Discovery)** and **T1059.001 (PowerShell)** according to the built-in detection rules.

No suspicious execution path, unauthorized user activity, or indicators of compromise were identified.

---

# Lab Validation

The Windows `net accounts` command was successfully executed from both Administrator Command Prompt and Administrator Windows PowerShell.

Both executions generated Sysmon Event ID 1 (Process Create) telemetry and were successfully collected by Wazuh.

The Command Prompt execution triggered Wazuh Rule **92031**, while the Windows PowerShell execution triggered Wazuh Rule **92033**, confirming that the built-in detection rules successfully identified the account discovery activity.

The generated alerts were successfully indexed into the `wazuh-alerts` index, confirming proper telemetry collection and detection within the Enterprise SOC Lab.

---

# Conclusion

The investigation confirmed that execution of the Windows `net accounts` command generated valid Sysmon Process Create telemetry and successfully triggered Wazuh detection rules.

The observed activity involved the legitimate Microsoft `net.exe` utility executed from both Administrator Command Prompt and Administrator Windows PowerShell. Wazuh identified the executions as Windows discovery activity through Rules **92031** and **92033**, mapping the behavior to the MITRE ATT&CK techniques **T1087 (Account Discovery)** and **T1059.001 (PowerShell)**.

Although attackers may use this command to gather password policy information during reconnaissance, the observed activity in this investigation represented expected administrative behavior performed in a controlled lab environment.

No further investigation or escalation is required based on the available evidence.

---

# Next Investigation

**18-Investigation-Net-Config-Workstation.md**

The next investigation focuses on monitoring the Windows `net config workstation` command to analyze workstation configuration discovery using Sysmon Process Create telemetry and Wazuh detection rules.

---

# Document Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | July 2026 | Rajeev Kumar | Initial release covering Administrator Command Prompt and Administrator Windows PowerShell executions of the `net accounts` command, Sysmon Event ID 1 telemetry, Wazuh Rules 92031 and 92033 analysis, MITRE ATT&CK mapping, and investigation findings. |