# Investigation-10 - Net LocalGroup Process Investigation


# Investigation Information
| Property | Details |
|----------|---------|
| **Investigation ID** | 10 |
| **Investigation Name** | Net LocalGroup Process Investigation |
| **Category** | Account Discovery |
| **Event Category** | Process Creation |
| **Platform** | Windows 10 |
| **Event Source** | Microsoft-Windows-Sysmon |
| **Event ID** | 1 (Process Create) | 
| **MITRE ATT&CK** | T1087 – Account Discovery |
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

The Objective of this investigation is to analyze the execution of the Windows **net localgroup** command using telemetry collected by Sysmon and Wahuz.

This investigation focuses on validating how Wazuh detects Windows localgroup account enumeration activities, analyzing the generated alerts, comparing different execution contexts (Command Prompt and Windows PowerShell), and determining whether the observed activity represents legitimate administrative behavior or potential account discovery performed by an attacker.

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

The Windows **net localgroup** command was executed on the Windows 10 endpoint to generate a Sysmon Process Create (Event ID 1), allowing the activity to be collected and analyzed in Wazuh.

Commands Executed:

```cmd
net localgroup
```

```text
Aliases for \\DESKTOP-8GB0J2A

-------------------------------------------------------------------------------
*Access Control Assistance Operators
*Administrators
*Backup Operators
*Cryptographic Operators
*Device Owners
*Distributed COM Users
*Event Log Readers
*Guests
*Hyper-V Administrators
*IIS_IUSRS
*Network Configuration Operators
*Performance Log Users
*Performance Monitor Users
*Power Users
*Remote Desktop Users
*Remote Management Users
*Replicator
*System Managed Accounts Group
*Users
The command completed successfully.

```

> 📸 **Screenshot Required**
>
> **Title:** Net localgroup Command Execution
>
> **Save As:** `screenshots/investigations/net-localgroup/01-command.png`

---

# Event Information

> **Note**
>
> The execution of the Windows `net localgroup` command generated Wazuh alerts because the activity matched built-in detection rules for Account Discovery.
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
| Command (CMD) | net localgroup |
| Command (PowerShell) | "C:\Windows\system32\net.exe" localgroup |
| Parent Process (CMD) | cmd.exe |
| Parent Process (PowerShell) | powershell.exe |
| Process Integrity | High (CMD), High (PowerShell) |
| User | DESKTOP-8GB0J2A\Oxhun |
| Detection Source | Wazuh Alerts |
| Wazuh Rule IDs | 92031, 92033 |
| Computer | DESKTOP-8GB0J2A |
| Agent | SOC-Win10 |

> 📸 **Screenshot Required**
>
> **Title:** Net LocalGroup Alert in Wazuh Discover
>
> **Save As:** `screenshots/investigations/net-localgroup/02-wazuh-event.png`

---

# Event Summary

The `net localgroup` command was executed twice during the investigation.

The first execution was performed from Command Prompt (Administrator) and generated a Sysmon Event ID 1, followed by Wazuh Alert Rule 92031.

The second execution was performed from an elevated Windows PowerShell (Administrator) session and generated another Sysmon Event ID 1, followed by Wazuh Alert Rule 92033.

Both executions launched the legitimate Microsoft `net.exe` binary located in `C:\Windows\System32`. Wazuh identified the activity as Account Discovery because the `net localgroup` command enumerates local group accounts. When executed from PowerShell, Wazuh additionally mapped the activity to PowerShell execution due to the parent process being `powershell.exe`.

---

# Event Analysis

## Execution 1 – Net LocalGroup (Administrator Command Prompt)

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\net.exe` |
| Original File Name | `net.exe` |
| Command Line | `net localgroup` |
| Description | `Net Command` |
| Parent Process | `C:\Windows\System32\cmd.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `7892` |
| Parent Process ID | `6136` |
| Integrity Level | `High` |
| Wazuh Rule | `92031` |
| Alert Level | `3` |
| Event Time | `2026-07-26 11:52:38.549 UTC` |

The first execution launched the legitimate Microsoft **net.exe** utility from an elevated **Command Prompt** using the `net localgroup` command. Sysmon recorded the process creation as **Event ID 1**, and Wazuh generated **Rule 92031 (Discovery activity executed)** because the command enumerated local groups on the system.

The process executed with **High** integrity, confirming that the command was launched from an Administrator Command Prompt. Wazuh mapped the activity to **MITRE ATT&CK T1087 – Account Discovery** because local group enumeration is commonly performed during the discovery phase by both administrators and attackers.

> 📸 **Screenshot Required**
>
> **Title:** Net LocalGroup Event Details (Administrator Command Prompt)
>
> **Save As:** `screenshots/investigations/net-localgroup/03-net-localgroup-cmd-event-details.png`

---

## Execution 2 – Net LocalGroup (Administrator Windows PowerShell)

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\net.exe` |
| Original File Name | `net.exe` |
| Command Line | `"C:\Windows\System32\net.exe" localgroup` |
| Description | `Net Command` |
| Parent Process | `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `4716` |
| Parent Process ID | `4604` |
| Integrity Level | `High` |
| Wazuh Rule | `92033` |
| Alert Level | `3` |
| Event Time | `2026-07-26 11:55:06.743 UTC` |

The second execution launched the same **net.exe** utility from an elevated **Windows PowerShell** session. Sysmon again generated **Event ID 1**, while Wazuh triggered **Rule 92033 (Discovery activity spawned via powershell execution)**.

Although the executed binary remained identical, Wazuh generated a different detection because the parent process changed from **cmd.exe** to **powershell.exe**. In addition to **MITRE ATT&CK T1087 (Account Discovery)**, this execution was also mapped to **T1059.001 (PowerShell)** because the command was executed through the Windows PowerShell interpreter.

> 📸 **Screenshot Required**
>
> **Title:** Net LocalGroup Event Details (Administrator Windows PowerShell)
>
> **Save As:** `screenshots/investigations/net-localgroup/04-net-localgroup-powershell-event-details.png`

---

# Timeline

| Time (UTC) | Activity |
|------------|----------|
| 2026-07-26 11:52:38 | `net localgroup` executed from Administrator Command Prompt |
| 2026-07-26 11:52:38 | Sysmon generated Event ID 1 |
| 2026-07-26 11:52:40 | Wazuh Rule **92031** generated an alert |
| 2026-07-26 11:52:40 | Event indexed into **wazuh-alerts** and **wazuh-archives** |
| 2026-07-26 11:55:06 | `net localgroup` executed from Administrator Windows PowerShell |
| 2026-07-26 11:55:06 | Sysmon generated Event ID 1 |
| 2026-07-26 11:55:07 | Wazuh Rule **92033** generated an alert |
| 2026-07-26 11:55:07 | Event indexed into **wazuh-alerts** and **wazuh-archives** |

---

# 5W Analysis

| Question | Analysis |
|----------|----------|
| **Who** | User **DESKTOP-8GB0J2A\Oxhun** executed the command. |
| **What** | The Windows `net localgroup` command was executed to enumerate local groups on the endpoint. |
| **When** | 2026-07-26 11:52:38 UTC (Administrator Command Prompt) and 2026-07-26 11:55:06 UTC (Administrator Windows PowerShell). |
| **Where** | Windows 10 endpoint monitored by Sysmon and Wazuh. |
| **Why** | The activity was intentionally performed to validate Wazuh detections for Windows local group enumeration within the Enterprise SOC Lab. |

---

# MITRE ATT&CK Mapping

| Tactic | Technique ID | Technique |
|---------|--------------|-----------|
| Discovery | T1087 | Account Discovery |
| Execution | T1059.001 | PowerShell *(PowerShell execution only)* |

---

# Investigation Findings

- The legitimate Microsoft `net.exe` binary was executed successfully.
- The executable was launched from the trusted `C:\Windows\System32` directory.
- The command was executed from both Administrator Command Prompt and Administrator Windows PowerShell.
- Both executions generated **Sysmon Event ID 1 (Process Create)**.
- Wazuh generated **Rule 92031** for the Administrator Command Prompt execution.
- Wazuh generated **Rule 92033** for the Administrator Windows PowerShell execution.
- Both executions were mapped to **MITRE ATT&CK T1087 (Account Discovery)**.
- The PowerShell execution was additionally mapped to **MITRE ATT&CK T1059.001 (PowerShell)**.
- Both executions ran with **High Integrity**, confirming administrative privileges.
- Events were successfully indexed into both **wazuh-alerts** and **wazuh-archives**.
- No suspicious execution path, binary replacement, or abnormal command-line arguments were identified.

---

# Final Verdict

| Property | Result |
|----------|--------|
| Classification | Benign Administrative Activity |
| Severity | Low |
| Risk | Informational |
| Detection Status | Successfully Detected by Wazuh |
| Escalation Required | No |
| Investigation Status | Closed |

---

# Analyst Notes

The Windows `net localgroup` command is a legitimate administrative utility used to display local security groups and manage group membership on Windows systems.

Because local group enumeration is commonly performed by both system administrators and threat actors during the Discovery phase, Wazuh monitors executions of this command and correlates them with Sysmon Process Create events.

This investigation demonstrated that Wazuh differentiates detections based on the execution context. The Administrator Command Prompt execution generated **Rule 92031**, whereas the Administrator Windows PowerShell execution generated **Rule 92033** and additionally mapped the activity to **MITRE ATT&CK T1059.001 (PowerShell)**.

SOC analysts should review the parent process, execution context, integrity level, command-line arguments, and user account before determining whether the activity represents legitimate administration or potential malicious reconnaissance.

---

# Lab Validation

The Windows `net localgroup` command was executed from both **Administrator Command Prompt** and **Administrator Windows PowerShell** to validate Wazuh detection capabilities.

Both executions generated **Sysmon Event ID 1 (Process Create)** and successfully triggered Wazuh detection rules.

The Administrator Command Prompt execution generated **Rule 92031 (Discovery activity executed)**, while the Administrator Windows PowerShell execution generated **Rule 92033 (Discovery activity spawned via powershell execution)**.

The generated events were successfully indexed into both the **wazuh-alerts** and **wazuh-archives** indices, confirming that the current Wazuh ruleset correctly detects Windows local group enumeration activities and distinguishes the execution context based on the parent process.

---

# Conclusion

The investigation confirmed that execution of the Windows `net localgroup` command represents legitimate administrative activity used to enumerate local groups on a Windows endpoint.

Both executions originated from the trusted Microsoft `net.exe` binary located in `C:\Windows\System32` and were performed with administrative privileges. Sysmon successfully recorded both executions as **Event ID 1**, while Wazuh generated **Rule 92031** for the Administrator Command Prompt execution and **Rule 92033** for the Administrator Windows PowerShell execution.

Although the command remained identical in both cases, Wazuh produced different detections because the parent process changed from **cmd.exe** to **powershell.exe**, demonstrating context-aware detection logic.

Based on the collected evidence, the activity is classified as **Benign Administrative Activity**, and no further investigation or escalation is required.

---

# Next Investigation

**11-Investigation-nslookup.md**

The next investigation analyzes the execution of the Windows `net view` command using Sysmon Process Create (Event ID 1) telemetry collected by Wazuh. The investigation focuses on monitoring network resource enumeration activities, analyzing generated alerts, and validating Wazuh detections related to Windows network discovery.

---

# Document Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | July 2026 | Rajeev Kumar | Initial release of the Net LocalGroup Process Investigation covering Administrator Command Prompt and Administrator Windows PowerShell executions, Sysmon Event ID 1 telemetry, Wazuh alert analysis, MITRE ATT&CK mapping, and investigation findings. |