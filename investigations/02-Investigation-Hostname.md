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

The `hostname` command was executed on the Windows 10 endpoint to generate a Sysmon Process Create (Event ID 1), allowing the activity to be collected and analyzed in Wazuh.

Command Executed:

```cmd
hostname
```

Observed Output:

```text
DESKTOP-8GB0J2A
```

> 📸 **Screenshot Required**
>
> **Title:** Hostname Command Execution
>
> **File:** `screenshots/investigations/hostname/01-command.png`

---

# Event Information

> **Note**
>
> The execution of the Windows `hostname` command does not generate a Wazuh alert because no detection rule is triggered for this activity.
>
> The event is collected by Sysmon (Event ID 1) and stored in the **wazuh-archives** index, where it can be investigated using Wazuh Discover.

| Property | Value |
|----------|-------|
| Event Type | Process Create |
| Event ID | 1 |
| Severity | Informational |
| Process | hostname.exe |
| User | DESKTOP-8GB0J2A\Oxhun |
| Computer | DESKTOP-8GB0J2A |
| Event Time (UTC) | 2026-07-23 05:06:43.455 |

> 📸 **Screenshot Required**
>
> **Title:** Hostname Event in Wazuh Discover
>
> **Save As:** `screenshots/investigations/hostname/02-wazuh-event.png`

---

# Event Summary

The event indicates that the Windows **hostname.exe** process was executed on the monitored endpoint.

The process was launched from **Command Prompt (cmd.exe)** by the logged-on user **DESKTOP-8GB0J2A\Oxhun**.

The executable was started from the default Windows System32 directory, and Sysmon successfully recorded the activity as **Event ID 1 (Process Create)**. The event was indexed in the **wazuh-archives** index for threat hunting and investigation purposes.

---

# Event Analysis

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\HOSTNAME.EXE` |
| Original File Name | `hostname.exe` |
| Command Line | `hostname` |
| Parent Process | `C:\Windows\System32\cmd.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `7704` |
| Parent Process ID | `7868` |
| Current Directory | `C:\Users\Oxhun\` |
| Integrity Level | Medium |
| Company | Microsoft Corporation |
| Event Time | 2026-07-23 05:06:43.455 UTC |

The event confirms that the legitimate Windows **hostname.exe** binary was executed from the trusted **System32** directory. The parent process (`cmd.exe`) and command line are consistent with normal interactive command execution, and no suspicious arguments or execution paths were observed.

> 📸 **Screenshot Required**
>
> **Title:** Hostname Event Details
>
> **Save As:** `screenshots/investigations/hostname/03-event-details.png`

---

# Timeline

| Time (UTC) | Activity |
|------------|----------|
| 2026-07-23 05:06:43.455 | User executed `hostname` |
| 2026-07-23 05:06:43.455 | Windows created the process |
| 2026-07-23 05:06:43.463 | Sysmon generated Event ID 1 |
| 2026-07-23 05:06:43.463 | Event became available for investigation in the `wazuh-archives` index. |

---

# 5W Analysis

| Question | Analysis |
|----------|----------|
| **Who** | DESKTOP-8GB0J2A\Oxhun executed the command. |
| **What** | The Windows `hostname.exe` process was created. |
| **When** | 2026-07-23 05:06:43 UTC. |
| **Where** | Windows endpoint **SOC-Win10**. |
| **Why** | To display the local computer hostname. |

---

# MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---------|-----------|----|
| Discovery | System Information Discovery | T1082 |

---

# Investigation Findings

The investigation confirmed the following:

- The process executed successfully.
- The executable is the legitimate Microsoft Windows binary.
- The executable path is the trusted Windows System32 directory.
- The parent process (`cmd.exe`) is expected.
- The command line contains only `hostname`.
- The activity was performed by the logged-on user.
- The event exists in the **wazuh-archives** index.
- No detection rule was triggered; therefore, no event was generated in the **wazuh-alerts** index.
- No suspicious indicators were identified.

---

# Final Verdict

| Property | Result |
|----------|--------|
| Classification | Benign Activity |
| Severity | Informational |
| Risk | Low |
| Escalation Required | No |
| Investigation Status | Closed |

---

# Analyst Notes

The execution of **hostname.exe** is a common administrative and diagnostic activity used to identify the local computer name.

Although the command is mapped to **MITRE ATT&CK T1082 (System Information Discovery)**, the observed activity appears legitimate because:

- The executable is the original Microsoft binary.
- It was executed from the trusted System32 directory.
- The parent process is Command Prompt.
- The command contains no suspicious arguments.
- No suspicious behavior was observed before or after the process execution.

This event is recorded by Sysmon and indexed in the **wazuh-archives** index. Since no Wazuh detection rule matches this activity, it does not generate an alert in the **wazuh-alerts** index.

---

# Conclusion

The event was investigated and determined to be **Benign**.

The investigation confirmed the normal execution of the Windows **hostname** command. The process was launched by the logged-on user from the legitimate Windows System32 directory, and no evidence of malicious activity, privilege escalation, or suspicious process behavior was identified.

The event was successfully collected by Sysmon and stored in the **wazuh-archives** index for threat hunting and forensic analysis.

---

# Next Investigation

**03-Investigation-Systeminfo.md**

The next investigation analyzes the execution of the Windows `systeminfo` command and the corresponding Sysmon Event ID 1 generated by Wazuh.

# Document Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | July 2026 | Rajeev Rock | Initial release of the Hostname investigation. |