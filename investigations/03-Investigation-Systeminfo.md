# Investigation 03 - Systeminfo

---

# Investigation Infomation

| Property | Details |
|----------|---------|
| **Investigation ID** | 03 |
| **Investigation Name** | Systeminfo Process Investigation |
| **Category** | Sysinfo Enumeration |
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

The objective of this investigation is to analyze the execution of the Windows **systeminfo** command using the telemetry collected by Sysmon and Wazuh.

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

The `systeminfo` command was executed on the Windows 10 endpoint to generate a Sysmon Process Create (Event ID 1), allowing the activity to be collected and analyzed in Wazuh.


Command Executed:

```cmd
systeminfo
```

Observed Output:

```text
Host Name:                 DESKTOP-8GB0J2A
OS Name:                   Microsoft Windows 10 Pro
OS Version:                10.0.19045 N/A Build 19045
OS Manufacturer:           Microsoft Corporation
OS Configuration:          Standalone Workstation
OS Build Type:             Multiprocessor Free
Registered Owner:          Oxhunter24@outlook.com
Registered Organization:
Product ID:                00330-80000-00000-AA213
Original Install Date:     09-01-2026, 23:15:04
System Boot Time:          23-07-2026, 10:29:38
System Manufacturer:       VMware, Inc.
System Model:              VMware20,1
System Type:               x64-based PC
Processor(s):              1 Processor(s) Installed.
                           [01]: Intel64 Family 6 Model 140 Stepping 1 GenuineIntel ~1805 Mhz
BIOS Version:              VMware, Inc. VMW201.00V.24006586.B64.2406042154, 04-06-2024
Windows Directory:         C:\Windows
System Directory:          C:\Windows\system32
Boot Device:               \Device\HarddiskVolume1
System Locale:             en-us;English (United States)
Input Locale:              00004009
Time Zone:                 (UTC+05:30) Chennai, Kolkata, Mumbai, New Delhi
Total Physical Memory:     2,047 MB
Available Physical Memory: 767 MB
Virtual Memory: Max Size:  7,167 MB
Virtual Memory: Available: 4,945 MB
Virtual Memory: In Use:    2,222 MB
Page File Location(s):     C:\pagefile.sys
Domain:                    WORKGROUP
Logon Server:              \\DESKTOP-8GB0J2A
Hotfix(s):                 12 Hotfix(s) Installed.
                           [01]: KB5066130
                           [02]: KB5022502
                           [03]: KB5011048
                           [04]: KB5015684
                           [05]: KB5020683
                           [06]: KB5033052
                           [07]: KB5072653
                           [08]: KB5071959
                           [09]: KB5014032
                           [10]: KB5025315
                           [11]: KB5066790
                           [12]: KB5071982
Network Card(s):           1 NIC(s) Installed.
                           [01]: Intel(R) 82574L Gigabit Network Connection
                                 Connection Name: Ethernet0
                                 DHCP Enabled:    Yes
                                 DHCP Server:     192.168.92.254
                                 IP address(es)
                                 [01]: 192.168.92.134
                                 [02]: fe80::c695:1dca:8571:8a3b
Hyper-V Requirements:      A hypervisor has been detected. Features required for Hyper-V will not be displayed.
```

> 📸 **Screenshot Required**
>
> **Title:** Systeminfo Command Execution
>
> **File:** `screenshots/investigations/systeminfo/01-command.png`

---

# Event Information

> **Note**
>
> The execution of the Windows `systeminfo` command does not generate a Wazuh alert because no detection rule is triggered for this activity.
>
> The event is collected by Sysmon (Event ID 1) and stored in the **wazuh-archives** index, where it can be investigated using Wazuh Discover.

| Property | Value |
|----------|-------|
| Event Type | Process Create |
| Event ID | 1 |
| Severity | Informational |
| Process : systeminfo.exe |
| Original File Name : sysinfo.exe |
| User | DESKTOP-8GB0J2A\Oxhun |
| Computer | DESKTOP-8GB0J2A |
| Event Time (UTC) | 2026-07-23 06:18:52.929 |

> 📸 **Screenshot Required**
>
> **Title:** Systeminfo Event in Wazuh Discover
>
> **Save As:** `screenshots/investigations/systeminfo/02-wazuh-event.png`

---

# Event Summary

The event indicates that the Windows **sysinfo.exe** process was executed on the monitored endpoint.

The process was launched from **Command Prompt (cmd.exe)** by the logged-on user **DESKTOP-8GB0J2A\Oxhun**.

The executable was started from the default Windows System32 directory, and Sysmon successfully recorded the activity as **Event ID 1 (Process Create)**. The event was indexed in the **wazuh-archives** index for threat hunting and investigation purposes.

---

# Event Analysis

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\systeminfo.exe` |
| Original File Name | `sysinfo.exe` |
| Command Line | `systeminfo` |
| Parent Process | `C:\Windows\System32\cmd.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `5748` |
| Parent Process ID | `7868` |
| Current Directory | `C:\Users\Oxhun\` |
| Integrity Level | Medium |
| Company | Microsoft Corporation |
| Event Time | 2026-07-23 06:18:52.929 UTC |

The event confirms that the legitimate Windows **systeminfo.exe** binary was executed from the trusted **System32** directory. The parent process (`cmd.exe`) and command line are consistent with normal interactive command execution, and no suspicious arguments or execution paths were observed.

> 📸 **Screenshot Required**
>
> **Title:** Systeminfo Event Details
>
> **Save As:** `screenshots/investigations/systeminfo/03-event-details.png`

---

# Timeline

| Time (UTC) | Activity |
|------------|----------|
| 2026-07-23 06:18:52.929 | User executed `systeminfo` |
| 2026-07-23 06:18:52.929 | Windows created the process |
| 2026-07-23 06:18:52.983 | Sysmon generated Event ID 1 |
| 2026-07-23 06:18:52.983 | Event became available for investigation in the `wazuh-archives` index. |
---

# 5W Analysis

| Question | Analysis |
|----------|----------|
| **Who** | DESKTOP-8GB0J2A\Oxhun executed the command. |
| **What** | The Windows `systeminfo.exe` process was created. |
| **When** | 2026-07-23 06:18:52 UTC. |
| **Where** | Windows endpoint **SOC-Win10**. |
| **Why** | To retrieve detailed system information about the local computer. |

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
- The original file name is `sysinfo.exe`.
- The command line contains only `systeminfo`.
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

The execution of **systeminfo.exe** is a common administrative and diagnostic activity used to retrieve detailed information about the local operating system and hardware.

Although the command is mapped to **MITRE ATT&CK T1082 (System Information Discovery)**, the observed activity appears legitimate because:

- The executable is the original Microsoft Windows binary.
- It was executed from the trusted Windows System32 directory.
- The parent process is Command Prompt (`cmd.exe`).
- The command line contains only `systeminfo`.
- No suspicious command-line arguments were supplied.
- No suspicious behavior was observed before or after the process execution.

The `systeminfo` command is frequently executed by:

- System administrators
- Helpdesk engineers
- Security analysts
- IT support personnel
- Automation and maintenance scripts

This event is recorded by Sysmon and indexed in the **wazuh-archives** index. Since no Wazuh detection rule matches this activity, it does not generate an alert in the **wazuh-alerts** index.

---

# Conclusion

The event was investigated and determined to be **Benign**.

The investigation confirmed the normal execution of the Windows **systeminfo** command. The process was launched by the logged-on user from the legitimate Windows System32 directory, and no evidence of malicious activity, privilege escalation, or suspicious process behavior was identified.

Although no Wazuh alert was generated, the archived Sysmon event provides sufficient telemetry for analysts to validate the execution and determine that the activity is benign.

---

# Next Investigation

**04-Investigation-Ipconfig.md**

The next investigation analyzes the execution of the Windows `ipconfig` command and the corresponding Sysmon Event ID 1 generated by Wazuh.

# Document Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | July 2026 | Rajeev Rock | Initial release of the Systeminfo investigation. |