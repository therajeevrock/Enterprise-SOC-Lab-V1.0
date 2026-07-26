# Investigation-09 - Net User Process Investigation


# Investigation Information
| Property | Details |
|----------|---------|
| **Investigation ID** | 09 |
| **Investigation Name** | Net User Process Investigation |
| **Category** | Account Discovery |
| **Event Category** | 
| **Platform** | Windows 10 |
| **Event Source** | Microsoft-Windows-Sysmon |
| **Event ID** | 1 (Process Create) |
| **MITRE ATT&CK** | T1087 Account Discovery  |
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

The objective of this investigation is to analyze the execution of the Windows **net user** command using telemetry collected by Sysmon and Wazuh.

This investigation focuses on validating how Wazuh detects Windows account enumeration activities, analyzing the generated alerts, comparing different execution contexts (Command Prompt and Windows PowerShell), and determining whether the observed activity represents legitimate administrative behavior or potential account discovery performed by an attacker.

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

The Windows **net user** command was executed on the Windows 10 endpoint to generate a Sysmon Process Create (Event ID 1), allowing the activity to be collected and analyzed in Wazuh.

Commands Executed:

```cmd
route
```

Observed Output:

```text
User accounts for \\DESKTOP-8GB0J2A

-------------------------------------------------------------------------------
Administrator            DefaultAccount           Guest
Oxhun                    sshd                     WDAGUtilityAccount
The command completed successfully.
```
> 📸 **Screenshot Required**
>
> **Title:** Net User Command Execution
>
> **Save As:** `screenshots/investigations/route/01-command.png`

---

# Event Information

> **Note**
>
> The execution of the Windows `net user` command generated Wazuh alerts because the activity matched built-in detection rules for Account Discovery.
>
> The process creation event was recorded by Sysmon (Event ID 1) and indexed in both the `wazuh-alerts` and `wazuh-archives` indices.

| Property | Value |
|----------|-------|
| Event Source | Microsoft-Windows-Sysmon |
| Event ID | 1 (Process Create) |
| Executable | C:\Windows\System32\net.exe |
| Original File Name | net.exe |
| Description | Net Command |
| Company | Microsoft Corporation |
| Command (CMD) | net user |
| Command (PowerShell) | "C:\Windows\system32\net.exe" user |
| Parent Process (CMD) | cmd.exe |
| Parent Process (PowerShell) | powershell.exe |
| Process Integrity | Medium (CMD), High (PowerShell) |
| User | DESKTOP-8GB0J2A\Oxhun |
| Detection Source | Wazuh Alerts |
| Wazuh Rule IDs | 92031, 92033 |

> 📸 **Screenshot Required**
>
> **Title:** Route Event in Wazuh Discover
>
> **Save As:** `screenshots/investigations/route/02-wazuh-event.png`

---

# Event Summary

The `net user` command was executed twice during the investigation.

The first execution was performed from Command Prompt and generated a Sysmon Event ID 1, followed by Wazuh Alert Rule 92031.

The second execution was performed from an elevated Windows PowerShell session and generated another Sysmon Event ID 1, followed by Wazuh Alert Rule 92033.

Both executions launched the legitimate Microsoft `net.exe` binary located in `C:\Windows\System32`. Wazuh identified the activity as Account Discovery because the `net user` command enumerates local user accounts. When executed from PowerShell, Wazuh additionally mapped the activity to PowerShell execution due to the parent process being `powershell.exe`.

---

# Event Analysis

## Execution 1 – Net User (Command Prompt)

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\net.exe` |
| Original File Name | `net.exe` |
| Command Line | `net user` |
| Description | `Net Command` |
| Parent Process | `C:\Windows\System32\cmd.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `8808` |
| Parent Process ID | `5340` |
| Integrity Level | `Medium` |
| Wazuh Rule | `92031` |
| Alert Level | `3` |
| Event Time | `2026-07-26 05:31:04.345 UTC` |

The first execution launched the legitimate Microsoft **net.exe** utility from **Command Prompt** using the `net user` command. Sysmon recorded the process creation as **Event ID 1**, and Wazuh generated **Rule 92031 (Discovery activity executed)** because the command enumerated local user accounts. The activity was mapped to **MITRE ATT&CK T1087 – Account Discovery**.

> 📸 **Screenshot Required**
>
> **Title:** Net User Event Details (Command Prompt)
>
> **Save As:** `screenshots/investigations/net-user/03-netuser-cmd-event-details.png`

---

## Execution 2 – Net User (Administrator PowerShell)

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\net.exe` |
| Original File Name | `net.exe` |
| Command Line | `"C:\Windows\System32\net.exe" user` |
| Description | `Net Command` |
| Parent Process | `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `7852` |
| Parent Process ID | `2840` |
| Integrity Level | `High` |
| Wazuh Rule | `92033` |
| Alert Level | `3` |
| Event Time | `2026-07-26 05:33:00.017 UTC` |

The second execution launched the same **net.exe** utility from an elevated **Windows PowerShell** session. The process executed with **High** integrity, indicating administrative privileges. Sysmon generated **Event ID 1**, and Wazuh triggered **Rule 92033 (Discovery activity spawned via PowerShell execution)**. In addition to **MITRE ATT&CK T1087 (Account Discovery)**, the activity was also mapped to **T1059.001 (PowerShell)** because the command was executed through the PowerShell interpreter.

> 📸 **Screenshot Required**
>
> **Title:** Net User Event Details (Administrator PowerShell)
>
> **Save As:** `screenshots/investigations/net-user/04-net-user-powershell-event-details.png`

---

# Timeline

| Time (UTC) | Activity |
|------------|----------|
| 2026-07-26 05:31:04 | `net user` executed from Command Prompt |
| 2026-07-26 05:31:04 | Sysmon generated Event ID 1 |
| 2026-07-26 05:31:06 | Wazuh Rule 92031 generated an alert |
| 2026-07-26 05:33:00 | `net user` executed from Administrator PowerShell |
| 2026-07-26 05:33:00 | Sysmon generated Event ID 1 |
| 2026-07-26 05:33:01 | Wazuh Rule 92033 generated an alert |

---

# 5W Analysis

| Question | Analysis |
|----------|----------|
| Who | User **DESKTOP-8GB0J2A\Oxhun** executed the command. |
| What | The `net user` command was executed to enumerate local user accounts. |
| When | 2026-07-26 05:31:04 UTC (Command Prompt) and 2026-07-26 05:33:00 UTC (Administrator PowerShell). |
| Where | Windows 10 endpoint monitored by Sysmon and Wazuh. |
| Why | The activity was performed to validate Wazuh detections and understand Account Discovery events within the SOC lab environment. |

---

# MITRE ATT&CK Mapping

| Tactic | Technique ID | Technique |
|---------|--------------|-----------|
| Discovery | T1087 | Account Discovery |
| Execution | T1059.001 | PowerShell (PowerShell execution only) |

---

# Investigation Findings

- The legitimate Microsoft `net.exe` binary was executed successfully.
- The command was executed from both Command Prompt and Administrator PowerShell.
- Sysmon generated Process Create (Event ID 1) for both executions.
- Wazuh generated Alert Rule **92031** for the Command Prompt execution.
- Wazuh generated Alert Rule **92033** for the PowerShell execution.
- Both events were mapped to **MITRE ATT&CK T1087 (Account Discovery)**.
- The PowerShell execution was additionally mapped to **T1059.001 (PowerShell)**.
- No malicious activity was identified during the investigation.

---

# Final Verdict

| Property | Result |
|----------|--------|
| Classification | Benign Administrative Activity |
| Severity | Low |
| Risk | Informational |
| Detection Status | Successfully Detected by Wazuh |
| Investigation Status | Closed |

---

# Analyst Notes

The `net user` command is a legitimate Windows administrative utility used to display local user accounts or retrieve detailed information about a specific account.

Because this command is commonly used by both administrators and attackers during the Account Discovery phase, Wazuh monitors its execution.

This investigation demonstrated that Wazuh considers the execution context when generating detections. Executing `net user` from Command Prompt triggered Rule 92031, while executing the same command from PowerShell triggered Rule 92033 and additionally mapped the activity to MITRE ATT&CK T1059.001 (PowerShell).

Analysts should always review the parent process, integrity level, user account, and execution context before determining whether the activity is legitimate or suspicious.

---

# Lab Validation

To validate the behavior of the Windows `net user` command within the lab environment, multiple execution contexts were tested.

The following scenarios were performed:

- Command Prompt (Standard User)
- Windows PowerShell (Administrator)

The following command was executed:

- `net user`

Both executions generated **Sysmon Process Create (Event ID 1)** events and successfully triggered Wazuh detection rules.

The Command Prompt execution generated **Wazuh Rule 92031 (Discovery activity executed)**, while the Administrator PowerShell execution generated **Wazuh Rule 92033 (Discovery activity spawned via powershell execution)**.

The generated events were successfully indexed in both the **wazuh-alerts** and **wazuh-archives** indices, confirming that the current Wazuh ruleset correctly detects Windows account enumeration activities and differentiates the execution context based on the parent process.

---

# Conclusion

The investigation determined that the execution of the Windows `net user` command represents **legitimate administrative activity** performed to enumerate local user accounts.

The observed processes were executed from the trusted `C:\Windows\System32` directory by the logged-on user through both **Command Prompt** and **Administrator Windows PowerShell**. The process metadata, execution path, parent-child relationships, integrity levels, and command-line arguments were consistent with expected Windows administrative behavior.

Sysmon successfully recorded both executions as **Event ID 1 (Process Create)**, and Wazuh generated alerts using **Rule 92031** for the Command Prompt execution and **Rule 92033** for the PowerShell execution. Although both executions performed the same command, Wazuh generated different detections based on the parent process, demonstrating context-aware detection capabilities.

Based on the available evidence, the activity is classified as **Benign Administrative Activity**, and no further investigation or escalation is required.

---

# Next Investigation

**10-Investigation-Net-Localgroup.md**

The next investigation analyzes the execution of the Windows `net localgroup` command and its corresponding Sysmon Process Create (Event ID 1) telemetry collected by Wazuh. The investigation focuses on monitoring local group enumeration activities, analyzing the generated process metadata, and validating Wazuh detections related to Windows account and group discovery.

---

# Document Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | July 2026 | Rajeev Kumar | Initial release of the Net User Process Investigation covering the `net user` command executions from Command Prompt and Administrator Windows PowerShell, including Sysmon telemetry, Wazuh alerts, MITRE ATT&CK mapping, and investigation findings. |