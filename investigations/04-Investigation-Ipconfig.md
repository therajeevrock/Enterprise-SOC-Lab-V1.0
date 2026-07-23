# Ivestigation 04 - Ipconfig

---

# Investigation Information

| Property | Details |
|----------|---------|
| **Investigation ID** | 04 |
| **Investigation Name** | Ipconfig Process Investigation |
| **Category** | Network Enumeration |
| **Event Category** | Process Creation |
| **Platform** | Windows 10 |
| **Event Source** | Microsoft-Windows-Sysmon |
| **Event ID** | 1 (Process Create) |
| **MITRE ATT&CK** | T1016 - System Network Configuration Discovery |
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

The objective of this investigation is to analyze the execution of the Windows **ipconfig** command using the telemetry collected by Sysmon and Wazuh.

The investigation focuses on verifying the process execution, identifying the executing user and parent process, and determining whether the activity is legitimate or requires further investigation.

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

The `ipconfig` command was executed on the Windows 10 endpoint to generate a Sysmon Process Create (Event ID 1), allowing the activity to be collected and analyzed in Wazuh.

Command Executed:

```cmd
ipconfig
```

Observed Output:

```text
Windows IP Configuration


Ethernet adapter Ethernet0:

   Connection-specific DNS Suffix  . : localdomain
   Link-local IPv6 Address . . . . . : fe80::c695:1dca:8571:8a3b%12
   IPv4 Address. . . . . . . . . . . : 192.168.92.134
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 192.168.92.2
```

> 📸 **Screenshot Required**
>
> **Title:** Ipconfig Command Execution
>
> **File:** `screenshots/investigations/ipconfig/01-command.png`

---

# Event Information

> **Note**
>
> The execution of the Windows `ipconfig` command does not generate a Wazuh alert because no detection rule is triggered for this activity.
>
> The event is collected by Sysmon (Event ID 1) and stored in the **wazuh-archives** index, where it can be investigated using Wazuh Discover.

| Property | Value |
|----------|-------|
| Event Type | Process Create |
| Event ID | 1 |
| Severity | Informational |
| Process | ipconfig.exe |
| User | DESKTOP-8GB0J2A\Oxhun |
| Computer | DESKTOP-8GB0J2A |
| Event Time (UTC) | 2026-07-23 08:05:11.032 |

> 📸 **Screenshot Required**
>
> **Title:** Ipconfig Event in Wazuh Discover
>
> **Save As:** `screenshots/investigations/ipconfig/02-wazuh-event.png`

---

# Event Summary

The event indicates that the Windows **ipconfig.exe** process was executed on the monitored endpoint.

The process was launched from **Command Prompt (cmd.exe)** by the logged-on user **DESKTOP-8GB0J2A\Oxhun**.

The executable was started from the default Windows System32 directory, and Sysmon successfully recorded the activity as **Event ID 1 (Process Create)**. The event was indexed in the **wazuh-archives** index for threat hunting and investigation purposes.

---

# Event Analysis

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\ipconfig.exe` |
| Original File Name | `ipconfig.exe` |
| Command Line | `ipconfig` |
| Parent Process | `C:\Windows\System32\cmd.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `9096` |
| Parent Process ID | `6584` |
| Current Directory | `C:\Users\Oxhun\` |
| Integrity Level | Medium |
| Company | Microsoft Corporation |
| Event Time | 2026-07-23 08:05:11.032 UTC |

The event confirms that the legitimate Windows **ipconfig.exe** binary was executed from the trusted **System32** directory. The parent process (`cmd.exe`) and command line are consistent with normal interactive command execution, and no suspicious arguments or execution paths were observed.

> 📸 **Screenshot Required**
>
> **Title:** Ipconfig Event Details
>
> **Save As:** `screenshots/investigations/ipconfig/03-event-details.png`

---

# Timeline

| Time (UTC) | Activity |
|------------|----------|
| 2026-07-23 08:05:11.032 | User executed `ipconfig`. |
| 2026-07-23 08:05:11.032 | Windows created the `ipconfig.exe` process. |
| 2026-07-23 08:05:11.078 | Sysmon generated Event ID 1. |
| 2026-07-23 08:05:11.078 | Event became available for investigation in the `wazuh-archives` index. |

---

# 5W Analysis

| Question | Analysis |
|----------|----------|
| **Who** | DESKTOP-8GB0J2A\Oxhun executed the command. |
| **What** | The Windows `ipconfig.exe` process was created. |
| **When** | 2026-07-23 08:05:11 UTC. |
| **Where** | Windows endpoint **SOC-Win10**. |
| **Why** | To display the current TCP/IP network configuration of the local computer. |

---

# MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---------|-----------|----|
| Discovery | System Network Configuration Discovery | T1016 |

---

# Investigation Findings

The investigation confirmed the following:

- The process executed successfully.
- The executable is the legitimate Microsoft Windows binary.
- The executable path is the trusted Windows System32 directory.
- The original file name is `ipconfig.exe`.
- The command line contains only `ipconfig`.
- The parent process (`cmd.exe`) is expected.
- The process was executed by the logged-on user.
- The integrity level is **Medium**, indicating execution under a standard user context.
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

The execution of **ipconfig.exe** is a common administrative and diagnostic activity used to retrieve the network configuration of the local computer.

Although the command is mapped to **MITRE ATT&CK T1016 (System Network Configuration Discovery)**, the observed activity appears legitimate because:

- The executable is the original Microsoft Windows binary.
- It was executed from the trusted Windows System32 directory.
- The parent process is Command Prompt (`cmd.exe`).
- The command line contains only `ipconfig`.
- No suspicious command-line arguments were supplied.
- No suspicious behavior was observed before or after the process execution.

The `ipconfig` command is frequently executed by:

- System administrators
- Network administrators
- Security analysts
- IT support personnel
- Troubleshooting and diagnostic scripts

This event is recorded by Sysmon and indexed in the **wazuh-archives** index. Since no Wazuh detection rule matches this activity, it does not generate an alert in the **wazuh-alerts** index.

---

# Conclusion

The event was investigated and determined to be **Benign**.

The investigation confirmed the normal execution of the Windows **ipconfig** command. The process was launched by the logged-on user from the legitimate Windows System32 directory, and no evidence of malicious activity, privilege escalation, or suspicious process behavior was identified.

Although no Wazuh alert was generated, the archived Sysmon event provides sufficient telemetry for analysts to validate the execution and determine that the activity is benign.

---

## Analyst Observation

During testing, another `ipconfig.exe` execution was observed in the `wazuh-alerts` index.

Further analysis showed that the process was executed by **VMware Tools** using `resume-vm-default.bat` under the `NT AUTHORITY\SYSTEM` account. This activity is unrelated to the manually executed `ipconfig` command analyzed in this investigation and was therefore excluded from the investigation scope.

This demonstrates the importance of validating the execution context (user, parent process, and command line) before determining whether an event is relevant to the current investigation.


> 📸 **Screenshot Required**
>
> **Title:** VMware Tools Generated `ipconfig /renew` Alert
>
> **Description:** Wazuh alert showing the automatic execution of `ipconfig /renew` by VMware Tools under the `NT AUTHORITY\SYSTEM` account. This event is unrelated to the manual `ipconfig` execution analyzed in this investigation.
>
> **Save As:** `screenshots/investigations/ipconfig/04-vmware-ipconfig-alert.png`

---

> 📸 **Screenshot Required**
>
> **Title:** VMware `ipconfig /renew` Event Details
>
> **Description:** Detailed event information showing the parent process (`resume-vm-default.bat`), command line (`ipconfig /renew`), executing user (`NT AUTHORITY\SYSTEM`), and other Sysmon process creation fields used to determine that the activity originated from VMware Tools.
>
> **Save As:** `screenshots/investigations/ipconfig/05-vmware-event-details.png`

---

# Next Investigation

**05-Investigation-Arp.md**

The next investigation analyzes the execution of the Windows `arp` command and the corresponding Sysmon Event ID 1 generated by Wazuh.

---

# Document Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | July 2026 | Rajeev Kumar | Initial release of the Ipconfig investigation. |