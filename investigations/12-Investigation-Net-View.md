# Investigation-12 - Net View Process Investigation


# Investigation Information
| Property | Details |
|----------|---------|
| **Investigation ID** | 12 |
| **Investigation Name** | Net View Process Investigation |
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

---

# Objective

The Objective of this investigation is to analyze the execution of the Windows **net view** command using telemetry collected by Sysmon and Wazuh.

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

The Windows **net view** command was executed on the Windows 10 endpoint to generate Sysmon Process Create (Event ID 1), allowing the activity to be collected and analyzed in Wazuh.

Commands Executed:

```cmd
net view
```

```text
System error 6118 has occurred.

The list of servers for this workgroup is not currently available
```

> 📸 **Screenshot Required**
>
> **Title:** Net View Command Execution
>
> **Save As:** `screenshots/investigations/net-view/01-command.png`

---

> **Note**
>
> The execution of the Windows `net view` command generated Wazuh alerts because the activity matched built-in detection rules for Account Discovery.
>
> The process creation event was recorded by Sysmon (Event ID 1) and indexed in both the `wazuh-alerts` and `wazuh-archives` indices.

---

# Event Information

| Property | Value |
|----------|-------|
| Event Source | Microsoft-Windows-Sysmon |
| Event ID | 1 (Process Create) |
| Executable | C:\Windows\System32\net.exe |
| Original File Name | net.exe |
| Description | Net Command |
| Company | Microsoft Corporation |
| Command (CMD) | net view |
| Command (PowerShell) | "C:\Windows\system32\net.exe" view |
| Parent Process (CMD) | cmd.exe |
| Parent Process (PowerShell) | powershell.exe |
| Process Integrity | High (CMD), High (PowerShell) |
| User | DESKTOP-8GB0J2A\Oxhun |
| Detection Source | wazuh-alerts |
| Wazuh Rule IDs | 92031(cmd), 92033(powershell) |
| Computer | DESKTOP-8GB0J2A |
| Agent | SOC-Win10 |

> 📸 **Screenshot Required**
>
> **Title:** Net View Event in Wazuh Discover 
>
> **Save As:** `screenshots/investigations/net-view/02-wazuh-event.png`

---

# Event Summary

The Windows `net view` command was executed twice during the investigation.

The first execution was performed from an Administrator Command Prompt, and the second execution was performed from an Administrator Windows PowerShell session.

Both executions generated Sysmon Process Create (Event ID 1) events and triggered Wazuh detection rules because the activity matched built-in discovery detections.

The executed binary was the legitimate Microsoft `net.exe` located in `C:\Windows\System32`.

---

# Event Analysis

## Execution 1 – Net View (Administrator Command Prompt)

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\net.exe` |
| Original File Name | `net.exe` |
| Command Line | `net view` |
| Description | `Net Command` |
| Parent Process | `C:\Windows\System32\cmd.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `1148` |
| Parent Process ID | `7040` |
| Integrity Level | `High` |
| Wazuh Rule | `92031` |
| Rule Description | `Discovery activity executed` |
| Event Time | `2026-07-28 01:18:57.294 UTC` |

The first execution launched the legitimate Microsoft **net.exe** utility from an elevated Command Prompt using the `net view` command.

Sysmon successfully recorded the process creation as **Event ID 1**, and Wazuh generated **Rule 92031**, identifying the activity as discovery-related execution.

The command was executed from the trusted `C:\Windows\System32` directory using High Integrity privileges, indicating legitimate administrative execution.

> 📸 **Screenshot Required**
>
> **Title:** Net View Event Details (Administrator Command Prompt)
>
> **Save As:** `screenshots/investigations/net-view/03-cmd-event.png`

---

## Execution 2 – Net View (Administrator Windows PowerShell)

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\net.exe` |
| Original File Name | `net.exe` |
| Command Line | `"C:\Windows\system32\net.exe" view` |
| Description | `Net Command` |
| Parent Process | `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `7080` |
| Parent Process ID | `6732` |
| Integrity Level | `High` |
| Wazuh Rule | `92033` |
| Rule Description | `Discovery activity spawned via powershell execution` |
| Event Time | `2026-07-28 01:36:17.243 UTC` |

The second execution launched the same Microsoft **net.exe** utility from an elevated Windows PowerShell session.

Sysmon generated another **Event ID 1**, and Wazuh generated **Rule 92033**, identifying the activity as discovery activity executed through PowerShell.

The command executed from the trusted `C:\Windows\System32` directory with High Integrity privileges. The detection was associated with both **Account Discovery (T1087)** and **PowerShell (T1059.001)** MITRE ATT&CK techniques.

> 📸 **Screenshot Required**
>
> **Title:** Net View Event Details (Administrator Windows PowerShell)
>
> **Save As:** `screenshots/investigations/net-view/04-powershell-event.png`

---

# Timeline

| Time (UTC) | Activity |
|------------|----------|
| 2026-07-28 01:18:57 | Administrator Command Prompt executed `net view` |
| 2026-07-28 01:18:57 | Sysmon generated Event ID 1 |
| 2026-07-28 01:18:58 | Wazuh generated Rule 92031 (Discovery activity executed) |
| 2026-07-28 01:36:17 | Administrator Windows PowerShell executed `net.exe view` |
| 2026-07-28 01:36:17 | Sysmon generated Event ID 1 |
| 2026-07-28 01:36:18 | Wazuh generated Rule 92033 (Discovery activity spawned via PowerShell execution) |

---

# 5W Analysis

| Question | Analysis |
|----------|----------|
| Who | `DESKTOP-8GB0J2A\Oxhun` |
| What | Executed the Windows `net view` command to enumerate available computers on the network. |
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
- The observed activity is consistent with administrative network discovery using the built-in Windows `net view` command.

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

The Windows `net view` command is a legitimate Microsoft utility used to enumerate computers and shared resources within a Windows network.

Although this command is frequently used by system administrators for network management and troubleshooting, it may also be used by attackers during the discovery phase to identify accessible systems before lateral movement.

In this investigation, the command was executed from trusted Windows binaries using High Integrity privileges. The activity generated Sysmon Process Create telemetry and triggered Wazuh detection rules because it matched built-in discovery detections. No evidence of malicious execution or abnormal behavior was observed.

---

# Lab Validation

The Windows `net view` command was successfully executed from both Administrator Command Prompt and Administrator Windows PowerShell.

Both executions generated Sysmon Event ID 1 (Process Create) telemetry and were successfully collected by Wazuh.

The Command Prompt execution triggered Wazuh Rule **92031**, while the PowerShell execution triggered Wazuh Rule **92033**, confirming that the built-in detection rules identified the activity as Windows discovery behavior.

The generated alerts were successfully indexed into the `wazuh-alerts` index, confirming proper telemetry collection and detection within the Enterprise SOC Lab.

---

# Conclusion

The investigation confirmed that execution of the Windows `net view` command generated valid Sysmon Process Create telemetry and successfully triggered Wazuh detection rules.

The observed activity involved the legitimate Microsoft `net.exe` utility executed from both Administrator Command Prompt and Administrator Windows PowerShell. Wazuh identified the executions as discovery activity through Rules **92031** and **92033**, mapping the behavior to the MITRE ATT&CK techniques **T1087 (Account Discovery)** and **T1059.001 (PowerShell)**.

Although `net view` can be abused by attackers during the reconnaissance phase to identify systems on a network, the observed activity in this investigation represented expected administrative behavior. No suspicious execution path, malicious arguments, or indicators of compromise were identified.

No further investigation or escalation is required based on the available evidence.

---

# Next Investigation

**13-Investigation-Net-Share.md**

The next investigation focuses on monitoring the Windows `net share` command to analyze local shared resource enumeration using Sysmon Process Create telemetry and Wazuh detection rules.

---

# Document Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | July 2026 | Rajeev Kumar | Initial release covering Administrator Command Prompt and Administrator Windows PowerShell executions of the `net view` command, Sysmon Event ID 1 telemetry, Wazuh Rules 92031 and 92033 analysis, MITRE ATT&CK mapping, and investigation findings. |