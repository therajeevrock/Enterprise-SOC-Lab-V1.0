# Investigation-13 - Net Share Process Investigation


# Investigation Information

| Property | Details |
|----------|---------|
| **Investigation ID** | 13 |
| **Investigation Name** | Net Share Process Investigation |
| **Category** | Network Share Discovery |
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
| `commands/MITRE-Reference.md` | MITRE ATT&CK techniques associated with Windows command-line activities used in this lab. |

---

# Objective

The objective of this investigation is to analyze the execution of the Windows **net share** command using telemetry collected by Sysmon and Wazuh.

The investigation demonstrates how Sysmon records process creation events and how Wazuh detects Windows discovery activity performed from both Command Prompt and Windows PowerShell.

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

The Windows **net share** command was executed on the Windows 10 endpoint from both Administrator Command Prompt and Administrator Windows PowerShell to generate Sysmon Process Create (Event ID 1), allowing the activity to be collected and analyzed in Wazuh.

## Command Prompt

```cmd
net share
```
```tex
Share name   Resource                        Remark

-------------------------------------------------------------------------------
C$           C:\                             Default share
IPC$                                         Remote IPC
ADMIN$       C:\Windows                      Remote Admin
The command completed successfully.
```

> 📸 **Screenshot Required**
>
> **Title:** Net Share Command Execution
>
> **Save As:** `screenshots/investigations/net-share/01-command.png`

---

> **Note**
>
> The execution of the Windows `net share` command generated Wazuh alerts because the activity matched built-in detection rules for Windows discovery activity.
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
| Command (CMD) | `net share` |
| Command (PowerShell) | `"C:\Windows\system32\net.exe" share` |
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
> **Title:** Net Share Event in Wazuh Discover
>
> **Save As:** `screenshots/investigations/net-share/02-wazuh-event.png`

---

# Event Summary

The Windows `net share` command was executed twice during the investigation.

The first execution was performed from an Administrator Command Prompt, and the second execution was performed from an Administrator Windows PowerShell session.

Both executions generated Sysmon Process Create (Event ID 1) events and triggered Wazuh detection rules because the activity matched built-in discovery detections.

The executed binary was the legitimate Microsoft `net.exe` located in `C:\Windows\System32`.

---

# Event Analysis

## Execution 1 – Net Share (Administrator Command Prompt)

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\net.exe` |
| Original File Name | `net.exe` |
| Command Line | `net share` |
| Description | `Net Command` |
| Parent Process | `C:\Windows\System32\cmd.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `5004` |
| Parent Process ID | `6904` |
| Integrity Level | `High` |
| Wazuh Rule | `92031` |
| Rule Description | `Discovery activity executed` |
| Event Time | `2026-07-28 03:05:40.533 UTC` |

The first execution launched the legitimate Microsoft **net.exe** utility from an elevated Command Prompt using the `net share` command.

Sysmon successfully recorded the process creation as **Event ID 1**, and Wazuh generated **Rule 92031**, identifying the activity as discovery-related execution.

The command was executed from the trusted `C:\Windows\System32` directory using High Integrity privileges, indicating legitimate administrative execution.

> 📸 **Screenshot Required**
>
> **Title:** Net Share Event Details (Administrator Command Prompt)
>
> **Save As:** `screenshots/investigations/net-share/03-cmd-event.png`

---

## Execution 2 – Net Share (Administrator Windows PowerShell)

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\net.exe` |
| Original File Name | `net.exe` |
| Command Line | `"C:\Windows\system32\net.exe" share` |
| Description | `Net Command` |
| Parent Process | `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `8560` |
| Parent Process ID | `8968` |
| Integrity Level | `High` |
| Wazuh Rule | `92033` |
| Rule Description | `Discovery activity spawned via powershell execution` |
| Event Time | `2026-07-28 03:04:19.807 UTC` |

The second execution launched the same Microsoft **net.exe** utility from an elevated Windows PowerShell session.

Sysmon generated another **Event ID 1**, and Wazuh generated **Rule 92033**, identifying the activity as discovery activity executed through PowerShell.

The command executed from the trusted `C:\Windows\System32` directory with High Integrity privileges. The detection was associated with both **Account Discovery (T1087)** and **PowerShell (T1059.001)** MITRE ATT&CK techniques.

> 📸 **Screenshot Required**
>
> **Title:** Net Share Event Details (Administrator Windows PowerShell)
>
> **Save As:** `screenshots/investigations/net-share/04-powershell-event.png`

---

# Event Summary

The Windows `net share` command was executed twice during the investigation.

The first execution was performed from an Administrator Command Prompt, and the second execution was performed from an Administrator Windows PowerShell session.

Both executions generated Sysmon Process Create (Event ID 1) events and triggered Wazuh detection rules because the activity matched built-in discovery detections.

The executed binary was the legitimate Microsoft `net.exe` located in `C:\Windows\System32`.

---

# Event Analysis

## Execution 1 – Net Share (Administrator Command Prompt)

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\net.exe` |
| Original File Name | `net.exe` |
| Command Line | `net share` |
| Description | `Net Command` |
| Parent Process | `C:\Windows\System32\cmd.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `5004` |
| Parent Process ID | `6904` |
| Integrity Level | `High` |
| Wazuh Rule | `92031` |
| Rule Description | `Discovery activity executed` |
| Event Time | `2026-07-28 03:05:40.533 UTC` |

The first execution launched the legitimate Microsoft **net.exe** utility from an elevated Command Prompt using the `net share` command.

Sysmon successfully recorded the process creation as **Event ID 1**, and Wazuh generated **Rule 92031**, identifying the activity as discovery-related execution.

The command was executed from the trusted `C:\Windows\System32` directory using High Integrity privileges, indicating legitimate administrative execution.

> 📸 **Screenshot Required**
>
> **Title:** Net Share Event Details (Administrator Command Prompt)
>
> **Save As:** `screenshots/investigations/net-share/03-cmd-event.png`

---

## Execution 2 – Net Share (Administrator Windows PowerShell)

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\net.exe` |
| Original File Name | `net.exe` |
| Command Line | `"C:\Windows\system32\net.exe" share` |
| Description | `Net Command` |
| Parent Process | `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `8560` |
| Parent Process ID | `8968` |
| Integrity Level | `High` |
| Wazuh Rule | `92033` |
| Rule Description | `Discovery activity spawned via powershell execution` |
| Event Time | `2026-07-28 03:04:19.807 UTC` |

The second execution launched the same Microsoft **net.exe** utility from an elevated Windows PowerShell session.

Sysmon generated another **Event ID 1**, and Wazuh generated **Rule 92033**, identifying the activity as discovery activity executed through PowerShell.

The command executed from the trusted `C:\Windows\System32` directory with High Integrity privileges. The detection was associated with both **Account Discovery (T1087)** and **PowerShell (T1059.001)** MITRE ATT&CK techniques.

> 📸 **Screenshot Required**
>
> **Title:** Net Share Event Details (Administrator Windows PowerShell)
>
> **Save As:** `screenshots/investigations/net-share/04-powershell-event.png`

---

# Timeline

| Time (UTC) | Activity |
|------------|----------|
| 2026-07-28 03:04:19 | Administrator Windows PowerShell executed `"C:\Windows\System32\net.exe" share` |
| 2026-07-28 03:04:19 | Sysmon generated Event ID 1 |
| 2026-07-28 03:04:20 | Wazuh generated Rule 92033 (Discovery activity spawned via PowerShell execution) |
| 2026-07-28 03:05:40 | Administrator Command Prompt executed `net share` |
| 2026-07-28 03:05:40 | Sysmon generated Event ID 1 |
| 2026-07-28 03:05:41 | Wazuh generated Rule 92031 (Discovery activity executed) |

---

# 5W Analysis

| Question | Analysis |
|----------|----------|
| Who | `DESKTOP-8GB0J2A\Oxhun` |
| What | Executed the Windows `net share` command to enumerate local shared resources on the system. |
| When | 28 July 2026 |
| Where | Windows 10 endpoint monitored by Sysmon and Wazuh |
| Why | Validate Sysmon telemetry collection and Wazuh detection of Windows discovery commands. |

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
- Wazuh generated Rule **92033** for the PowerShell execution.
- Both executions ran with High Integrity privileges.
- No suspicious execution path or abnormal command-line arguments were identified.
- The observed activity is consistent with administrative enumeration of local Windows shared resources using the built-in `net share` command.

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

The Windows `net share` command is a legitimate Microsoft administrative utility used to display, create, modify, or remove shared resources on a Windows system.

During security investigations, attackers may use this command to identify available shared folders before attempting lateral movement or unauthorized access. For this reason, security monitoring solutions frequently classify the command as Discovery activity.

In this investigation, the command was executed from trusted Windows binaries using High Integrity privileges. Sysmon successfully recorded the Process Create events, and Wazuh generated Rules **92031** and **92033** because the activity matched built-in discovery detections. No evidence of malicious execution or abnormal behavior was identified.

---

# Lab Validation

The Windows `net share` command was successfully executed from both Administrator Command Prompt and Administrator Windows PowerShell.

Both executions generated Sysmon Event ID 1 (Process Create) telemetry and were successfully collected by Wazuh.

The Command Prompt execution triggered Wazuh Rule **92031**, while the PowerShell execution triggered Wazuh Rule **92033**, confirming that the built-in detection rules identified the activity as Windows discovery behavior.

The generated alerts were successfully indexed into the `wazuh-alerts` index, confirming proper telemetry collection and detection within the Enterprise SOC Lab.

---

# Conclusion

The investigation confirmed that execution of the Windows `net share` command generated valid Sysmon Process Create telemetry and successfully triggered Wazuh detection rules.

The observed activity involved the legitimate Microsoft `net.exe` utility executed from both Administrator Command Prompt and Administrator Windows PowerShell. Wazuh identified the executions as discovery activity through Rules **92031** and **92033**, mapping the behavior to the MITRE ATT&CK techniques **T1087 (Account Discovery)** and **T1059.001 (PowerShell)**.

Although `net share` can be abused by attackers to identify accessible shared resources during the reconnaissance phase, the observed activity in this investigation represented expected administrative behavior. No suspicious execution path, malicious arguments, or indicators of compromise were identified.

No further investigation or escalation is required based on the available evidence.

---

# Next Investigation

**14-Investigation-Net-Use.md**

The next investigation focuses on monitoring the Windows `net use` command to analyze network drive connections and remote share access using Sysmon Process Create telemetry and Wazuh detection rules.

---

# Document Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | July 2026 | Rajeev Kumar | Initial release covering Administrator Command Prompt and Administrator Windows PowerShell executions of the `net share` command, Sysmon Event ID 1 telemetry, Wazuh Rules 92031 and 92033 analysis, MITRE ATT&CK mapping, and investigation findings. |