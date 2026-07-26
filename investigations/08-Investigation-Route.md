# Investigation 08 - Route Process Investigation

# Investigation Information

| Property | Details |
|----------|---------|
| **Investigation ID** | 08 |
| **Investigation Name** | Route Process Investigation |
| **Category** | Network Configuration Discovery |
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

The objective of this investigation is to analyze the execution of the Windows **route** command using telemetry collected by Sysmon and Wazuh.

The investigation focuses on verifying the process execution, identifying the executing user and parent process, and determining whether the activity represents normal administrative behavior or suspicious network configuration discovery.

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

The Windows **route** & **route print** command was executed on the Windows 10 endpoint to generate a Sysmon Process Create (Event ID 1), allowing the activity to be collected and analyzed in Wazuh.

Commands Executed:

```cmd
route
```

Observed Output:

```text
Manipulates network routing tables.

ROUTE [-f] [-p] [-4|-6] command [destination]
                  [MASK netmask]  [gateway] [METRIC metric]  [IF interface]

  -f           Clears the routing tables of all gateway entries.  If this is
               used in conjunction with one of the commands, the tables are
               cleared prior to running the command.

  -p           When used with the ADD command, makes a route persistent across
               boots of the system. By default, routes are not preserved
               when the system is restarted. Ignored for all other commands,
               which always affect the appropriate persistent routes.

  -4           Force using IPv4.

  -6           Force using IPv6.

  command      One of these:
                 PRINT     Prints  a route
                 ADD       Adds    a route
                 DELETE    Deletes a route
                 CHANGE    Modifies an existing route
  destination  Specifies the host.
  MASK         Specifies that the next parameter is the 'netmask' value.
  netmask      Specifies a subnet mask value for this route entry.
               If not specified, it defaults to 255.255.255.255.
  gateway      Specifies gateway.
  interface    the interface number for the specified route.
  METRIC       specifies the metric, ie. cost for the destination.

All symbolic names used for destination are looked up in the network database
file NETWORKS. The symbolic names for gateway are looked up in the host name
database file HOSTS.

If the command is PRINT or DELETE. Destination or gateway can be a wildcard,
(wildcard is specified as a star '*'), or the gateway argument may be omitted.

If Dest contains a * or ?, it is treated as a shell pattern, and only
matching destination routes are printed. The '*' matches any string,
and '?' matches any one char. Examples: 157.*.1, 157.*, 127.*, *224*.

Pattern match is only allowed in PRINT command.
Diagnostic Notes:
    Invalid MASK generates an error, that is when (DEST & MASK) != DEST.
    Example> route ADD 157.0.0.0 MASK 155.0.0.0 157.55.80.1 IF 1
             The route addition failed: The specified mask parameter is invalid. (Destination & Mask) != Destination.

Examples:

    > route PRINT
    > route PRINT -4
    > route PRINT -6
    > route PRINT 157*          .... Only prints those matching 157*

    > route ADD 157.0.0.0 MASK 255.0.0.0  157.55.80.1 METRIC 3 IF 2
             destination^      ^mask      ^gateway     metric^    ^
                                                         Interface^
      If IF is not given, it tries to find the best interface for a given
      gateway.
    > route ADD 3ffe::/32 3ffe::1

    > route CHANGE 157.0.0.0 MASK 255.0.0.0 157.55.80.5 METRIC 2 IF 2

      CHANGE is used to modify gateway and/or metric only.

    > route DELETE 157.0.0.0
    > route DELETE 3ffe::/32=

```

> 📸 **Screenshot Required**
>
> **Title:** Route Command Execution
>
> **Save As:** `screenshots/investigations/route/01-command.png`


Commands Executed:

```cmd
route print
```

Observed Output:

```text
===========================================================================
Interface List
 12...00 0c 29 0b f9 e2 ......Intel(R) 82574L Gigabit Network Connection
  1...........................Software Loopback Interface 1
===========================================================================

IPv4 Route Table
===========================================================================
Active Routes:
Network Destination        Netmask          Gateway       Interface  Metric
          0.0.0.0          0.0.0.0     192.168.92.2   192.168.92.134     25
        127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
        127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
  127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
     192.168.92.0    255.255.255.0         On-link    192.168.92.134    281
   192.168.92.134  255.255.255.255         On-link    192.168.92.134    281
   192.168.92.255  255.255.255.255         On-link    192.168.92.134    281
        224.0.0.0        240.0.0.0         On-link         127.0.0.1    331
        224.0.0.0        240.0.0.0         On-link    192.168.92.134    281
  255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
  255.255.255.255  255.255.255.255         On-link    192.168.92.134    281
===========================================================================
Persistent Routes:
  None

IPv6 Route Table
===========================================================================
Active Routes:
 If Metric Network Destination      Gateway
  1    331 ::1/128                  On-link
 12    281 fe80::/64                On-link
 12    281 fe80::c695:1dca:8571:8a3b/128
                                    On-link
  1    331 ff00::/8                 On-link
 12    281 ff00::/8                 On-link
===========================================================================
Persistent Routes:
  None
```

> 📸 **Screenshot Required**
>
> **Title:** Route print Command Execution
>
> **Save As:** `screenshots/investigations/route/02-print-command.png`

---

# Event Information

> **Note**
>
> The execution of the Windows `route` command does not generate a Wazuh alert because no detection rule matches this activity.
>
> The process creation event is collected by Sysmon (Event ID 1) and stored in the **wazuh-archives** index, where it can be used for threat hunting and forensic investigations.

| Property | Value |
|----------|-------|
| Event Type | Process Create |
| Event ID | 1 |
| Severity | Informational |
| Process | route.exe |
| User | DESKTOP-8GB0J2A\Oxhun |
| Computer | DESKTOP-8GB0J2A |
| Event Time (UTC) | 2026-07-26 02:06:29.984 |

> 📸 **Screenshot Required**
>
> **Title:** Route Event in Wazuh Discover
>
> **Save As:** `screenshots/investigations/route/03-wazuh-event.png`

---

# Event Summary

The investigation analyzed three executions of the Windows **route.exe** utility on the monitored endpoint.

The first execution was launched from **Command Prompt**, the second execution was launched from an **elevated Windows PowerShell** session, and the third execution used the **route print** command from a standard Windows PowerShell session.

All executions used the legitimate Microsoft **route.exe** binary to display routing information for the local system.

Sysmon successfully recorded all executions as **Event ID 1 (Process Create)**, and the events were stored in the **wazuh-archives** index for threat hunting and forensic analysis. No corresponding Wazuh alerts were generated.

---

# Event Analysis

## Execution 1 – route (Command Prompt)

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\ROUTE.EXE` |
| Original File Name | `route.exe` |
| Command Line | `route` |
| Description | `TCP/IP Route Command` |
| Parent Process | `C:\Windows\System32\cmd.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `4572` |
| Parent Process ID | `5056` |
| Integrity Level | `Medium` |
| Event Time | `2026-07-26 02:06:29.984 UTC` |

The first execution launched the legitimate Microsoft **route.exe** utility from **Command Prompt**. The command displayed the local routing table and generated a Sysmon Event ID 1.

> 📸 **Screenshot Required**
>
> **Title:** Route Event Details (Command Prompt)
>
> **Save As:** `screenshots/investigations/route/04-route-cmd-event-details.png`

---

## Execution 2 – route (Administrator PowerShell)

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\ROUTE.EXE` |
| Original File Name | `route.exe` |
| Command Line | `"C:\Windows\System32\ROUTE.EXE"` |
| Description | `TCP/IP Route Command` |
| Parent Process | `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `7872` |
| Parent Process ID | `7688` |
| Integrity Level | `High` |
| Event Time | `2026-07-26 02:12:52.746 UTC` |

The second execution launched the same **route.exe** utility from an elevated **Windows PowerShell** session. The process executed with **High** integrity, indicating administrative privileges, while the executable, execution path, and user remained consistent with legitimate administrative activity.

> 📸 **Screenshot Required**
>
> **Title:** Route Event Details (Administrator PowerShell)
>
> **Save As:** `screenshots/investigations/route/05-route-powershell-event-details.png`

---

---

## Execution 3 – route print (Windows PowerShell)

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\ROUTE.EXE` |
| Original File Name | `route.exe` |
| Command Line | `"C:\Windows\System32\ROUTE.EXE" print` |
| Description | `TCP/IP Route Command` |
| Parent Process | `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `8620` |
| Parent Process ID | `7724` |
| Integrity Level | `Medium` |
| Event Time | `2026-07-26 02:36:12.928 UTC` |

The third execution used the **route print** command from a standard Windows PowerShell session. This command displays the complete IPv4 and IPv6 routing tables and is commonly used during network diagnostics and routing analysis. The process executed successfully and generated a Sysmon Event ID 1.

> 📸 **Screenshot Required**
>
> **Title:** Route Print Event Details (PowerShell)
>
> **Save As:** `screenshots/investigations/route/06-route-print-event-details.png`

---

# Timeline

| Time (UTC) | Activity |
|------------|----------|
| 2026-07-26 02:06:29.984 | User executed the `route` command from Command Prompt. |
| 2026-07-26 02:06:29.994 | Sysmon generated Event ID 1 for the Command Prompt execution. |
| 2026-07-26 02:12:52.746 | User executed the `route` command from Administrator PowerShell. |
| 2026-07-26 02:12:52.761 | Sysmon generated Event ID 1 for the Administrator PowerShell execution. |
| 2026-07-26 02:36:12.928 | User executed the `route print` command from Windows PowerShell. |
| 2026-07-26 02:36:12.936 | Sysmon generated Event ID 1 for the `route print` execution. |
| 2026-07-26 02:36:12.936 | All events were stored in the `wazuh-archives` index for investigation. |

---

# 5W Analysis

| Question | Analysis |
|----------|----------|
| **Who** | DESKTOP-8GB0J2A\Oxhun executed the command. |
| **What** | The Windows `route.exe` process was created. |
| **When** | 2026-07-26 02:06:29 UTC, 2026-07-26 02:12:52 UTC, and 2026-07-26 02:36:12 UTC. |
| **Where** | Windows endpoint **SOC-Win10**. |
| **Why** | To display the system routing table and network route information for network configuration analysis and troubleshooting. |

---

# MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---------|-----------|----|
| Discovery | System Network Configuration Discovery | T1016 |

---

# Investigation Findings

The investigation confirmed the following:

- The Windows `route.exe` utility was executed successfully.
- Three executions of the Windows `route.exe` utility were observed during the investigation.
- The following command variants were executed:
  - `route`
  - `route` (Administrator PowerShell)
  - `route print`
- All executions used the legitimate Microsoft Windows `route.exe` binary.
- The executable was launched from the trusted `C:\Windows\System32\` directory.
- All processes were initiated by the logged-on user `DESKTOP-8GB0J2A\Oxhun`.
- The parent processes included both **Command Prompt (`cmd.exe`)** and **Windows PowerShell (`powershell.exe`)**.
- The executions were observed with both **Medium** and **High** integrity levels, depending on the execution context.
- Sysmon successfully recorded all executions as **Event ID 1 (Process Create)**.
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

The Windows **route.exe** utility is a legitimate command-line tool used to display and manage the Windows IP routing table.

During this investigation, the `route` command was executed from **Command Prompt** and **Administrator Windows PowerShell**, while the `route print` command was executed from a standard **Windows PowerShell** session.

The `route` command displays command usage information when executed without parameters, whereas `route print` displays the complete IPv4 and IPv6 routing tables. These commands are commonly used by system administrators, network engineers, SOC analysts, and incident responders to verify network routes, troubleshoot connectivity issues, and analyze routing configurations.

Although these commands are associated with **MITRE ATT&CK T1016 – System Network Configuration Discovery**, no suspicious behavior was observed during this investigation.

The current Wazuh configuration records all executions in the **wazuh-archives** index through Sysmon Event ID 1. Since no Wazuh detection rule matches these activities, no alerts are generated in the **wazuh-alerts** index.

---

# Lab Validation

To validate the behavior of the `route` command within the lab environment, multiple execution contexts were tested.

The following scenarios were performed:

- Command Prompt (Standard User)
- Windows PowerShell (Standard User)
- Windows PowerShell (Administrator)

The following command variants were executed:

- `route`
- `route print`

All executions generated Sysmon Process Create (Event ID 1) events, which were successfully indexed in the **wazuh-archives** index. No corresponding events were generated in the **wazuh-alerts** index, confirming that the current Wazuh ruleset treats these executions as normal administrative activity.

---

# Conclusion

The investigation determined that the execution of the Windows `route` and `route print` commands represents **normal Windows administrative activity**.

The observed processes were executed from the legitimate Windows System32 directory by the logged-on user through Command Prompt and Windows PowerShell. The process metadata, execution context, parent-child relationships, integrity levels, and command-line arguments were consistent with expected network administration and troubleshooting activities.

Based on the available evidence, the activity is classified as **Benign**, and no further investigation or escalation is required.

---

# Next Investigation

**09-Investigation-Net-User.md**

The next investigation analyzes the execution of the Windows `net user` command and its corresponding Sysmon Process Create (Event ID 1) telemetry collected by Wazuh.

---

# Document Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | July 2026 | Rajeev Kumar | Initial release of the Route Process Investigation covering the `route` and `route print` command executions from multiple execution contexts. |