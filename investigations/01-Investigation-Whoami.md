# Investigation 01 - Whoami

---

# Investigation Information

| Property | Details |
|----------|---------|
| **Investigation ID** | 01 |
| **Investigation Name** | Whoami Process Investigation |
| **Category** | User Enumeration |
| **Platform** | Windows 10 |
| **Event Source** | Microsoft-Windows-Sysmon |
| **Event ID** | 1 (Process Create) |
| **MITRE ATT&CK** | T1033 – System Owner / User Discovery |
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

The objective of this investigation is to analyze the execution of the Windows **whoami** command using the telemetry collected by Sysmon and Wazuh.

The investigation focuses on verifying the process execution, identifying the user and parent process, and determining whether the activity is legitimate or requires further investigation.

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

The `whoami` command was executed on the Windows 10 endpoint to generate a Sysmon Process Create (Event ID 1), allowing the activity to be collected and analyzed in Wazuh.

Command Executed:

```cmd
whoami
```

Observed Output:

```text
desktop-8gb0j2a\oxhun
```

> 📸 **Screenshot Required**
>
> **Title:** Whoami Command Execution
>
> **File:** `screenshots/investigations/whoami/01-command.png`

---

# Event Information

| Property | Value |
|----------|-------|
| Event Type | Process Create |
| Event ID | 1 |
| Severity | Informational |
| Process | whoami.exe |
| User | DESKTOP-8GB0J2A\Oxhun |
| Computer | DESKTOP-8GB0J2A |
| Event Time (UTC) | 2026-07-15 10:47:37.594 |

> 📸 **Screenshot Required**
>
> **Title:** Whoami Alert in Wazuh
>
> **Save As:** `screenshots/investigations/whoami/02-wazuh-event.png`

---

# Event Summary

The alert indicates that the Windows **whoami.exe** process was executed on the monitored endpoint.

The process was launched from **Command Prompt (cmd.exe)** by the logged-on user **DESKTOP-8GB0J2A\Oxhun**.

The executable was started from the default Windows System32 directory, and Sysmon successfully recorded the activity as **Event ID 1 (Process Create)**. The event was indexed in the **wazuh-archives** index for threat hunting and investigation purposes.

---

# Event Analysis

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\whoami.exe` |
| Command Line | `whoami` |
| Parent Process | `C:\Windows\System32\cmd.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `3200` |
| Parent Process ID | `6208` |
| Current Directory | `C:\Users\Oxhun\` |
| Integrity Level | Medium |
| Company | Microsoft Corporation |
| Event Time | 2026-07-15 10:47:37.594 UTC |

The event confirms that the legitimate Windows **whoami.exe** binary was executed from the trusted **System32** directory. The parent process (`cmd.exe`) and command line are consistent with normal interactive command execution, and no suspicious arguments or execution paths were observed.

> 📸 **Screenshot Required**
>
> **Title:** Whoami Event Details
>
> **Save As:** `screenshots/investigations/whoami/03-event-details.png`

---

# Timeline

| Time (UTC) | Activity |
|------------|----------|
| 2026-07-15 10:47:37.594 | User executed `whoami` |
| 2026-07-15 10:47:37.594 | Windows created the process |
| 2026-07-15 10:47:37.611 | Sysmon generated Event ID 1 |
| 2026-07-15 10:47:38.584 | Wazuh received and indexed the event into `wazuh-archives` |

---

# 5W Analysis

| Question | Analysis |
|----------|----------|
| **Who** | DESKTOP-8GB0J2A\Oxhun executed the command. |
| **What** | The Windows `whoami.exe` process was created. |
| **When** | 2026-07-15 10:47:37 UTC. |
| **Where** | Windows endpoint **SOC-Win10**. |
| **Why** | To identify the currently logged-on user. |

---

# MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---------|-----------|----|
| Discovery | System Owner / User Discovery | T1033 |

---

# Investigation Findings

The investigation confirmed the following:

- The process executed successfully.
- The executable is the legitimate Microsoft Windows binary.
- The executable path is the trusted Windows System32 directory.
- The parent process (`cmd.exe`) is expected.
- The command line contains only `whoami`.
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

The execution of **whoami.exe** is common during system administration, troubleshooting, and security investigations.

Although the command is mapped to **MITRE ATT&CK T1033 (Discovery)**, the observed activity appears legitimate because:

- The executable is the original Microsoft binary.
- It was executed from the trusted System32 directory.
- The parent process is Command Prompt.
- No suspicious behavior was observed before or after the process execution.

This event is recorded by Sysmon and indexed in the **wazuh-archives** index. Since no Wazuh detection rule matches this activity, it does not generate an alert in the **wazuh-alerts** index.


---

# Conclusion

The alert was investigated and determined to be **Benign**.

The event represents the normal execution of the Windows **whoami** command. No evidence of malicious activity, privilege escalation, or suspicious process behavior was identified during the investigation.

The event was successfully collected by Sysmon and stored in the **wazuh-archives** index for threat hunting and forensic analysis.

---

# Next Investigation

**02-Investigation-Hostname.md**

The next investigation analyzes the execution of the Windows `hostname` command and the corresponding Sysmon Event ID 1 generated by Wazuh.

---

# Document Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | July 2026 | Rajeev Rock | Initial release of the Whoami investigation. |