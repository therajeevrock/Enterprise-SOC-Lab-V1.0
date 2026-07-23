# Investigation Information

| Property | Details |
|----------|---------|
| **Investigation ID** | 05 |
| **Investigation Name** | ARP Process Investigation |
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

The objective of this investigation is to analyze the execution of the Windows **arp** command using telemetry collected by Sysmon and Wazuh.

The investigation focuses on verifying the process execution, identifying the executing user and parent process, and determining whether the activity represents normal administrative behavior or suspicious network discovery.

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

The Windows **arp** command was executed on the Windows 10 endpoint to generate a Sysmon Process Create (Event ID 1), allowing the activity to be collected and analyzed in Wazuh.

Commands Executed:

```cmd
arp
arp -a
```

Observed Output:

```text
PS C:\Windows\system32> arp

Displays and modifies the IP-to-Physical address translation tables used by
address resolution protocol (ARP).

ARP -s inet_addr eth_addr [if_addr]
ARP -d inet_addr [if_addr]
ARP -a [inet_addr] [-N if_addr] [-v]

  -a            Displays current ARP entries by interrogating the current
                protocol data.  If inet_addr is specified, the IP and Physical
                addresses for only the specified computer are displayed.  If
                more than one network interface uses ARP, entries for each ARP
                table are displayed.
  -g            Same as -a.
  -v            Displays current ARP entries in verbose mode.  All invalid
                entries and entries on the loop-back interface will be shown.
  inet_addr     Specifies an internet address.
  -N if_addr    Displays the ARP entries for the network interface specified
                by if_addr.
  -d            Deletes the host specified by inet_addr. inet_addr may be
                wildcarded with * to delete all hosts.
  -s            Adds the host and associates the Internet address inet_addr
                with the Physical address eth_addr.  The Physical address is
                given as 6 hexadecimal bytes separated by hyphens. The entry
                is permanent.
  eth_addr      Specifies a physical address.
  if_addr       If present, this specifies the Internet address of the
                interface whose address translation table should be modified.
                If not present, the first applicable interface will be used.
Example:
  > arp -s 157.55.85.212   00-aa-00-62-c6-09  .... Adds a static entry.
  > arp -a                                    .... Displays the arp table.
PS C:\Windows\system32> arp -a

Interface: 192.168.92.134 --- 0xc
  Internet Address      Physical Address      Type
  192.168.92.2          00-50-56-f2-03-ab     dynamic
  192.168.92.140        00-0c-29-46-7c-77     dynamic
  192.168.92.254        00-50-56-f9-5c-a8     dynamic
  192.168.92.255        ff-ff-ff-ff-ff-ff     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.251           01-00-5e-00-00-fb     static
  224.0.0.252           01-00-5e-00-00-fc     static
  255.255.255.255       ff-ff-ff-ff-ff-ff     static
```

> 📸 **Screenshot Required**
>
> **Title:** ARP Command Execution
>
> **Save As:** `screenshots/investigations/arp/01-command.png`

---

# Event Information

> **Note**
>
> The execution of the Windows `arp` command does not generate a Wazuh alert because no detection rule matches this activity.
>
> The process creation event is collected by Sysmon (Event ID 1) and stored in the **wazuh-archives** index, where it can be used for threat hunting and forensic investigations.

| Property | Value |
|----------|-------|
| Event Type | Process Create |
| Event ID | 1 |
| Severity | Informational |
| Process | arp.exe |
| User | DESKTOP-8GB0J2A\Oxhun |
| Computer | DESKTOP-8GB0J2A |
| Event Time (UTC) | 2026-07-23 09:08:53.010 |

> 📸 **Screenshot Required**
>
> **Title:** ARP Event in Wazuh Discover
>
> **Save As:** `screenshots/investigations/arp/02-wazuh-event.png`

---

# Event Summary

The investigation analyzed two executions of the Windows **arp.exe** utility on the monitored endpoint.

The first execution used the default `arp` command, while the second execution used the `arp -a` option to display the complete ARP cache.

Both executions were launched from **Command Prompt (cmd.exe)** by the logged-on user **DESKTOP-8GB0J2A\Oxhun**.

Sysmon successfully recorded both executions as **Event ID 1 (Process Create)**, and the events were stored in the **wazuh-archives** index for threat hunting and forensic analysis. No corresponding Wazuh alerts were generated.

---

# Event Analysis

## Execution 1 – arp

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\ARP.EXE` |
| Original File Name | `arp.exe` |
| Command Line | `arp` |
| Description | `TCP/IP Arp Command` |
| Parent Process | `C:\Windows\System32\cmd.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `3772` |
| Parent Process ID | `6584` |
| Integrity Level | `Medium` |
| Event Time | `2026-07-23 09:08:53.010 UTC` |

The first execution launched the legitimate Microsoft **arp.exe** utility without any command-line arguments. This command displays basic ARP information and is commonly used for network diagnostics.

> 📸 **Screenshot Required**
>
> **Title:** ARP Command Event Details
>
> **Save As:** `screenshots/investigations/arp/03-arp-event-details.png`

---

## Execution 2 – arp -a

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\ARP.EXE` |
| Original File Name | `arp.exe` |
| Command Line | `arp -a` |
| Description | `TCP/IP Arp Command` |
| Parent Process | `C:\Windows\System32\cmd.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `3852` |
| Parent Process ID | `6584` |
| Integrity Level | `Medium` |
| Event Time | `2026-07-23 09:12:55.689 UTC` |

The second execution used the **`-a`** option, which displays all entries in the ARP cache. The executable, execution path, parent process, and user remained unchanged, indicating another legitimate interactive execution.

> 📸 **Screenshot Required**
>
> **Title:** ARP -a Event Details
>
> **Save As:** `screenshots/investigations/arp/04-arp-a-event-details.png`

---

# Timeline

| Time (UTC) | Activity |
|------------|----------|
| 2026-07-23 09:08:53.010 | User executed the `arp` command. |
| 2026-07-23 09:08:53.130 | Sysmon generated Event ID 1 for the `arp` execution. |
| 2026-07-23 09:12:55.689 | User executed the `arp -a` command. |
| 2026-07-23 09:12:55.768 | Sysmon generated Event ID 1 for the `arp -a` execution. |
| 2026-07-23 09:12:55.768 | Both events were stored in the `wazuh-archives` index for investigation. |

---

# 5W Analysis

| Question | Analysis |
|----------|----------|
| **Who** | DESKTOP-8GB0J2A\Oxhun executed the command. |
| **What** | The Windows `arp.exe` process was created. |
| **When** | 2026-07-23 09:08:53 UTC. |
| **Where** | Windows endpoint **SOC-Win10**. |
| **Why** | To display or manage the Address Resolution Protocol (ARP) cache used for IP-to-MAC address resolution. |

---

# MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---------|-----------|----|
| Discovery | System Network Configuration Discovery | T1016 |

---

# Investigation Findings

The investigation confirmed the following:

- The Windows `arp.exe` utility was executed successfully.
- Two command variants were observed during the investigation:
  - `arp`
  - `arp -a`
- Both executions used the legitimate Microsoft Windows `arp.exe` binary.
- The executable was launched from the trusted `C:\Windows\System32\` directory.
- Both processes were initiated by the logged-on user `DESKTOP-8GB0J2A\Oxhun`.
- The parent process for both executions was `cmd.exe`.
- Both executions ran with **Medium** integrity, indicating execution under a standard user context.
- Sysmon successfully recorded both executions as **Event ID 1 (Process Create)**.
- The events were stored in the **wazuh-archives** index.
- No Wazuh detection rules were triggered; therefore, no corresponding events were generated in the **wazuh-alerts** index.
- No suspicious indicators, abnormal execution paths, or malicious command-line arguments were identified.

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

The Windows **arp.exe** utility is a legitimate command-line tool used to display and manage the Address Resolution Protocol (ARP) cache.

During this investigation, both `arp` and `arp -a` were executed and analyzed. The telemetry collected by Sysmon confirmed that both commands originated from the trusted Windows System32 directory and were launched interactively through **Command Prompt (`cmd.exe`)** by the logged-on user.

The `arp` command displays ARP information, while `arp -a` displays all ARP cache entries for every network interface. Both commands are commonly used by system administrators, network engineers, SOC analysts, and IT support personnel during network troubleshooting and incident investigations.

Although these commands are associated with **MITRE ATT&CK T1016 – System Network Configuration Discovery**, no suspicious behavior was observed in this investigation.

The current Wazuh configuration records both executions in the **wazuh-archives** index through Sysmon Event ID 1. Since no Wazuh detection rule matches these activities, no alerts are generated in the **wazuh-alerts** index.

---

# Lab Validation

To validate the behavior of the `arp` command within the lab environment, multiple execution contexts were tested.

The following scenarios were performed:

- Command Prompt (Standard User)
- Command Prompt (Administrator)
- Windows PowerShell (Standard User)
- Windows PowerShell (Administrator)

In each environment, both `arp` and `arp -a` were executed.

All executions generated Sysmon Process Create (Event ID 1) events, which were successfully indexed in **wazuh-archives**. No corresponding events were generated in **wazuh-alerts**, confirming that the current Wazuh ruleset treats these executions as normal administrative activity.

---

# Conclusion

The investigation determined that both `arp` and `arp -a` executions represent **normal Windows administrative activity**.

The observed processes were executed from the legitimate Windows System32 directory by the logged-on user through Command Prompt. The process metadata, execution context, parent-child relationship, and command-line arguments were consistent with expected system administration and network diagnostic tasks.

Based on the available evidence, the activity is classified as **Benign**, and no further investigation or escalation is required.

---

# Next Investigation

**06-Investigation-Netstat.md**

The next investigation analyzes the execution of the Windows `netstat` command and its corresponding Sysmon Process Create (Event ID 1) telemetry collected by Wazuh.

---

# Document Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | July 2026 | Rajeev Kumar | Initial release of the ARP Process Investigation covering both `arp` and `arp -a` command executions. |