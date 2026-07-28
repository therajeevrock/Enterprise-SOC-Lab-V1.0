# Investigation-11 - NSLookup Process Investigation


# Investigation Information
| Property | Details |
|----------|---------|
| **Investigation ID** | 11 |
| **Investigation Name** | NSLookup Process Investigation |
| **Category** | Network Configuration Discovery |
| **Event Category** | Process Creation |
| **Platform** | Windows 10 |
| **Event Source** | Microsoft-Windows-Sysmon |
| **Event ID** | 1 (Process Create) |
| **MITRE ATT&CK** | T1016 – System Network Configuration Discovery |
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

The Objective of this investigation is to analyze the execution of the Windows **nslookup** command using telemetry collected by Sysmon and Wazuh.

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

The Windows **nslookup** command was executed on the Windows 10 endpoint to generate a Sysmon Process Create (Event ID 1), allowing the activity to be collected and analyzed in Wazuh.

Commands Executed:

```cmd
nslookup
```

```text
Default Server:  UnKnown
Address:  192.168.92.2

> 192.168.91.254
Server:  UnKnown
Address:  192.168.92.2

*** UnKnown can't find 192.168.91.254: Non-existent domain
>
```
> 📸 **Screenshot Required**
>
> **Title:** NSLookup Command Execution
>
> **Save As:** `screenshots/investigations/nslookup/01-command.png`

---

> **Note**
>
> The execution of the Windows `nslookup` command did not generate a Wazuh alert because the activity did not match any built-in Wazuh detection rule.
>
> The process creation event was successfully recorded by Sysmon (Event ID 1) and indexed in the `wazuh-archives` index.
>

---

# Event Information

| Property | Value |
|----------|-------|
| Event Source | Microsoft-Windows-Sysmon |
| Event ID | 1 (Process Create) |
| Executable | C:\Windows\System32\nslookup.exe |
| Original File Name | nslookup.exe |
| Description | nslookup |
| Company | Microsoft Corporation |
| Command (CMD) | nslookup |
| Command (PowerShell) | "C:\Windows\System32\nslookup.exe" |
| Parent Process (CMD) | cmd.exe |
| Parent Process (PowerShell) | powershell.exe |
| Process Integrity | High (CMD), High (PowerShell) |
| User | DESKTOP-8GB0J2A\Oxhun |
| Detection Source | wazuh-archives |
| Wazuh Rule IDs | No matching Wazuh rule |
| Computer | DESKTOP-8GB0J2A |
| Agent | SOC-Win10 |

> 📸 **Screenshot Required**
>
> **Title:** NSLookup Event in Wazuh Discover 
>
> **Save As:** `screenshots/investigations/nslookup/02-wazuh-event.png`

---

# Event Summary

The Windows `nslookup` command was executed twice during the investigation.

The first execution was performed from an Administrator Command Prompt, and the second execution was performed from an Administrator Windows PowerShell session.

Both executions generated Sysmon Process Create (Event ID 1) events and were successfully collected by Wazuh. However, no Wazuh detection rule matched the activity, so the events were indexed only in the `wazuh-archives` index.

The executed binary was the legitimate Microsoft `nslookup.exe` located in `C:\Windows\System32`.

---

# Event Analysis

## Execution 1 – NSLookup (Administrator Command Prompt)

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\nslookup.exe` |
| Original File Name | `nslookup.exe` |
| Command Line | `nslookup` |
| Description | `nslookup` |
| Parent Process | `C:\Windows\System32\cmd.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `5656` |
| Parent Process ID | `1128` |
| Integrity Level | `High` |
| Wazuh Rule | `None` |
| Event Time | `2026-07-27 01:46:13.470 UTC` |

The first execution launched the legitimate Microsoft **nslookup.exe** utility from an elevated Command Prompt.

Sysmon successfully recorded the process creation as **Event ID 1**, and Wazuh collected the telemetry in the **wazuh-archives** index.

No Wazuh detection rule matched this activity because it represented normal execution of a legitimate Windows networking utility.

> 📸 Screenshot Required
>
> **Title:** NSLookup Event Details (Administrator Command Prompt)
>
> **Save As:** `screenshots/investigations/nslookup/03-nslookup-cmd-event-details.png`

---

## Execution 2 – NSLookup (Administrator Windows PowerShell)

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\nslookup.exe` |
| Original File Name | `nslookup.exe` |
| Command Line | `"C:\Windows\System32\nslookup.exe"` |
| Description | `nslookup` |
| Parent Process | `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `3728` |
| Parent Process ID | `6444` |
| Integrity Level | `High` |
| Wazuh Rule | `None` |
| Event Time | `2026-07-27 01:49:00.630 UTC` |

The second execution launched the same Microsoft **nslookup.exe** utility from an elevated Windows PowerShell session.

Sysmon generated another **Event ID 1**, and Wazuh archived the event without generating an alert because no detection rule matched the activity.

> 📸 Screenshot Required
>
> **Title:** NSLookup Event Details (Administrator Windows PowerShell)
>
> **Save As:** `screenshots/investigations/nslookup/03-nslookup-powershell-event-details.png`

---

# Timeline

| Time (UTC) | Activity |
|------------|----------|
| 2026-07-27 01:46:13 | Administrator Command Prompt executed `nslookup` |
| 2026-07-27 01:46:13 | Sysmon generated Event ID 1 |
| 2026-07-27 01:46:14 | Event indexed into `wazuh-archives` |
| 2026-07-27 01:49:00 | Administrator Windows PowerShell executed `nslookup.exe` |
| 2026-07-27 01:49:00 | Sysmon generated Event ID 1 |
| 2026-07-27 01:49:01 | Event indexed into `wazuh-archives` |

---

# 5W Analysis

| Question | Analysis |
|----------|----------|
| **Who** | DESKTOP-8GB0J2A\Oxhun |
| **What** | Executed the Windows `nslookup` command. |
| **When** | 27 July 2026 |
| **Where** | Windows 10 endpoint monitored by Sysmon and Wazuh |
| **Why** | Validate Sysmon telemetry collection and Wazuh event ingestion. |

---

# MITRE ATT&CK Mapping

| Tactic | Technique ID | Technique |
|---------|--------------|-----------|
| Discovery | T1016 | System Network Configuration Discovery |

---

# Investigation Findings

- Legitimate Microsoft `nslookup.exe` executed successfully.
- Executed from the trusted `C:\Windows\System32` directory.
- Executed from both Administrator Command Prompt and Administrator Windows PowerShell.
- Both executions generated Sysmon Event ID 1.
- Both executions ran with High Integrity.
- No Wazuh detection rule matched the activity.
- Events were successfully indexed into the `wazuh-archives` index.
- No suspicious execution path or abnormal command-line arguments were identified.
- The executed binary was digitally signed by Microsoft Corporation.

---

# Final Verdict

| Property | Result |
|----------|--------|
| Classification | Benign Administrative Activity |
| Severity | Informational |
| Risk | Low |
| Detection Status | Archived Only (No Alert Generated) |
| Escalation Required | No |
| Investigation Status | Closed |

---

# Analyst Notes

The Windows `nslookup` command is a legitimate Microsoft utility used for DNS name resolution and troubleshooting.

Although attackers may use this command during reconnaissance, the observed activity showed no indicators of malicious behavior.

Because no Wazuh detection rule matched the execution, the events were stored only in the `wazuh-archives` index. SOC analysts can still use the Sysmon telemetry for timeline reconstruction and threat hunting.

---

# Lab Validation

The Windows `nslookup` command was executed from both Administrator Command Prompt and Administrator Windows PowerShell.

Both executions generated Sysmon Event ID 1 and were successfully collected by Wazuh.

No Wazuh alert was generated because the activity did not match any built-in detection rule.

The events were successfully stored in the `wazuh-archives` index.

---

# Conclusion

The investigation confirmed that execution of the Windows `nslookup` command generated valid Sysmon Process Create telemetry and that Wazuh successfully collected and archived the events.

Although no detection rule matched the activity, the collected telemetry remains valuable for forensic analysis, timeline reconstruction, and threat hunting.

The observed executions represent legitimate administrative activity, and no escalation is required.

---

# Next Investigation

**12-Investigation-Net-View.md**

The next investigation focuses on monitoring the Windows `net view` command to analyze network resource discovery using Sysmon Process Create telemetry collected by Wazuh.

# Document Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | July 2026 | Rajeev Kumar | Initial release covering Administrator Command Prompt and Administrator Windows PowerShell executions of `nslookup`, Sysmon Event ID 1 telemetry, Wazuh event analysis, MITRE ATT&CK mapping, and investigation findings. |