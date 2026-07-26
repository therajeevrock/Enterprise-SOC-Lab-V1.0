# Investigation-06 - Netstat Process Investigation


# Investigation Information

| Property | Details |
|----------|---------|
| **Investigation ID** | 06 |
| **Investigation Name** | Netstat Process Investigation |
| **Category** | Network Enumeration |
| **Event Category** | Process Creation |
| **Platform** | Windows 10 |
| **Event Source** | Microsoft-Windows-Sysmon |
| **Event ID** | 1 (Process Create) |
| **MITRE ATT&CK** | T1049 - System Network Connections Discovery |
| **Difficulty** | Beginner |

# Related References

The following reference documents provide additional information related to this investigation.

| Document | Purpose |
|----------|---------|
| `commands/Windows-Command-Reference.md` | Windows command reference used throughout the Enterprise SOC Lab investigations. |
| `commands/Wazuh-Search-Queries.md` | Wazuh Threat Hunting search queries used to locate investigation events. |

---

# Objective

The objective of this investigation is to analyze the execution of the Windows **netstat** command using telemetry collected by Sysmon and Wazuh.

The investigation focuses on verifying the process execution, identifying the executing user and parent process, and determining whether the activity represents normal administrative behavior or suspicious network connection discovery.

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

# Lab Execution

The Windows **netstat** command was executed on the Windows 10 endpoint to generate a Sysmon Process Create (Event ID 1), allowing the activity to be collected and analyzed in Wazuh.

Commands Executed:

```cmd
netstat
netstat -ano

PS C:\Windows\system32> netstat

Active Connections

  Proto  Local Address          Foreign Address        State
  TCP    192.168.92.134:49677   4.213.25.241:https     ESTABLISHED
  TCP    192.168.92.134:49714   4.213.25.241:https     ESTABLISHED
  TCP    192.168.92.134:49802   192.168.92.140:1514    ESTABLISHED
  TCP    192.168.92.134:49826   52.168.117.174:https   TIME_WAIT
  TCP    192.168.92.134:49831   49.44.141.200:https    ESTABLISHED
  TCP    192.168.92.134:49832   a2-19-212-212:https    ESTABLISHED
  TCP    192.168.92.134:49838   40.104.66.178:https    ESTABLISHED
  TCP    192.168.92.134:49839   40.104.66.178:https    ESTABLISHED
  TCP    192.168.92.134:49840   52.168.117.174:https   ESTABLISHED
  TCP    192.168.92.134:49843   150.171.27.11:https    TIME_WAIT
  TCP    192.168.92.134:49844   74.226.64.245:https    ESTABLISHED
  TCP    192.168.92.134:49845   20.190.146.38:https    ESTABLISHED
  TCP    192.168.92.134:49846   a23-53-243-208:https   ESTABLISHED

PS C:\Windows\system32> netstat -ano

Active Connections

  Proto  Local Address          Foreign Address        State           PID
  TCP    0.0.0.0:80             0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:135            0.0.0.0:0              LISTENING       596
  TCP    0.0.0.0:445            0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:3389           0.0.0.0:0              LISTENING       1160
  TCP    0.0.0.0:5040           0.0.0.0:0              LISTENING       1344
  TCP    0.0.0.0:7680           0.0.0.0:0              LISTENING       7184
  TCP    0.0.0.0:49664          0.0.0.0:0              LISTENING       872
  TCP    0.0.0.0:49665          0.0.0.0:0              LISTENING       744
  TCP    0.0.0.0:49666          0.0.0.0:0              LISTENING       1200
  TCP    0.0.0.0:49667          0.0.0.0:0              LISTENING       1176
  TCP    0.0.0.0:49668          0.0.0.0:0              LISTENING       2088
  TCP    0.0.0.0:49684          0.0.0.0:0              LISTENING       864
  TCP    192.168.92.134:139     0.0.0.0:0              LISTENING       4
  TCP    192.168.92.134:49677   4.213.25.241:443       ESTABLISHED     1176
  TCP    192.168.92.134:49714   4.213.25.241:443       ESTABLISHED     5088
  TCP    192.168.92.134:49802   192.168.92.140:1514    ESTABLISHED     2804
  TCP    192.168.92.134:49831   49.44.141.200:443      CLOSE_WAIT      1616
  TCP    192.168.92.134:49832   2.19.212.212:443       CLOSE_WAIT      1616
  TCP    192.168.92.134:49838   40.104.66.178:443      ESTABLISHED     1616
  TCP    192.168.92.134:49839   40.104.66.178:443      ESTABLISHED     1616
  TCP    192.168.92.134:49849   40.79.150.120:443      ESTABLISHED     2492
  TCP    192.168.92.134:49850   54.192.151.84:443      ESTABLISHED     2492
  TCP    192.168.92.134:49854   2.19.212.206:443       ESTABLISHED     2492
  TCP    192.168.92.134:49860   52.178.17.235:443      TIME_WAIT       0
  TCP    192.168.92.134:49862   49.44.195.121:443      ESTABLISHED     2492
  TCP    192.168.92.134:49863   49.44.195.121:443      ESTABLISHED     2492
  TCP    192.168.92.134:49864   150.171.110.85:443     CLOSE_WAIT      2492
  TCP    192.168.92.134:49865   150.171.28.10:443      ESTABLISHED     2492
  TCP    192.168.92.134:49866   13.107.139.11:443      ESTABLISHED     5088
  TCP    192.168.92.134:49867   4.150.223.97:443       ESTABLISHED     8176
  TCP    192.168.92.134:49868   52.104.132.53:443      ESTABLISHED     5088
  TCP    192.168.92.134:49869   52.178.17.235:443      ESTABLISHED     7328
  TCP    [::]:80                [::]:0                 LISTENING       4
  TCP    [::]:135               [::]:0                 LISTENING       596
  TCP    [::]:445               [::]:0                 LISTENING       4
  TCP    [::]:3389              [::]:0                 LISTENING       1160
  TCP    [::]:7680              [::]:0                 LISTENING       7184
  TCP    [::]:49664             [::]:0                 LISTENING       872
  TCP    [::]:49665             [::]:0                 LISTENING       744
  TCP    [::]:49666             [::]:0                 LISTENING       1200
  TCP    [::]:49667             [::]:0                 LISTENING       1176
  TCP    [::]:49668             [::]:0                 LISTENING       2088
  TCP    [::]:49684             [::]:0                 LISTENING       864
  TCP    [::1]:42050            [::]:0                 LISTENING       7328
  UDP    0.0.0.0:123            *:*                                    7792
  UDP    0.0.0.0:161            *:*                                    2468
  UDP    0.0.0.0:500            *:*                                    1176
  UDP    0.0.0.0:3389           *:*                                    1160
  UDP    0.0.0.0:4500           *:*                                    1176
  UDP    0.0.0.0:5050           *:*                                    1344
  UDP    0.0.0.0:5353           *:*                                    1632
  UDP    0.0.0.0:5355           *:*                                    1632
  UDP    0.0.0.0:51965          *:*                                    2492
  UDP    0.0.0.0:57994          *:*                                    2492
  UDP    127.0.0.1:1900         *:*                                    5036
  UDP    127.0.0.1:49987        *:*                                    5036
  UDP    127.0.0.1:50788        *:*                                    1176
  UDP    192.168.92.134:137     *:*                                    4
  UDP    192.168.92.134:138     *:*                                    4
  UDP    192.168.92.134:1900    *:*                                    5036
  UDP    192.168.92.134:49986   *:*                                    5036
  UDP    [::]:123               *:*                                    7792
  UDP    [::]:161               *:*                                    2468
  UDP    [::]:500               *:*                                    1176
  UDP    [::]:3389              *:*                                    1160
  UDP    [::]:4500              *:*                                    1176
  UDP    [::]:5353              *:*                                    1632
  UDP    [::]:5355              *:*                                    1632
  UDP    [::1]:1900             *:*                                    5036
  UDP    [::1]:49985            *:*                                    5036
  UDP    [fe80::c695:1dca:8571:8a3b%12]:1900  *:*                                    5036
  UDP    [fe80::c695:1dca:8571:8a3b%12]:49984  *:*                                    5036

```

> 📸 **Screenshot Required**
>
> **Title:** Netstat Command Execution
>
> **Save As:** `screenshots/investigations/netstat/01-command.png`

---

# Event Information

> **Note**
>
> The execution of the Windows `netstat` command does not generate a Wazuh alert because no detection rule matches this activity.
>
> The process creation event is collected by Sysmon (Event ID 1) and stored in the **wazuh-archives** index, where it can be used for threat hunting and forensic investigations.

| Property | Value |
|----------|-------|
| Event Type | Process Create |
| Event ID | 1 |
| Severity | Informational |
| Process | netstat.exe |
| User | DESKTOP-8GB0J2A\Oxhun |
| Computer | DESKTOP-8GB0J2A |
| Event Time (UTC) | 2026-07-25 02:31:07.230 |

> 📸 **Screenshot Required**
>
> **Title:** Netstat Event in Wazuh Discover
>
> **Save As:** `screenshots/investigations/netstat/02-wazuh-event.png`

---

# Event Summary

The investigation analyzed two executions of the Windows **netstat.exe** utility on the monitored endpoint.

The first execution used the default `netstat` command, while the second execution used the `netstat -ano` option to display all active network connections, listening ports, and their associated Process IDs (PIDs).

Both executions were launched from **Command Prompt (cmd.exe)** by the logged-on user **DESKTOP-8GB0J2A\Oxhun**.

Sysmon successfully recorded both executions as **Event ID 1 (Process Create)**, and the events were stored in the **wazuh-archives** index for threat hunting and forensic analysis. No corresponding Wazuh alerts were generated.

---

# Event Analysis

## Execution 1 – netstat

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\NETSTAT.EXE` |
| Original File Name | `netstat.exe` |
| Command Line | `netstat` |
| Description | `TCP/IP Netstat Command` |
| Parent Process | `C:\Windows\System32\cmd.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `252` |
| Parent Process ID | `4144` |
| Integrity Level | `Medium` |
| Event Time | `2026-07-25 02:31:07.230 UTC` |

The first execution launched the legitimate Microsoft **netstat.exe** utility without any command-line arguments. This command displays active network connections and listening ports and is commonly used for network diagnostics and troubleshooting.

> 📸 **Screenshot Required**
>
> **Title:** Netstat Command Event Details
>
> **Save As:** `screenshots/investigations/netstat/03-netstat-event-details.png`

---

## Execution 2 – netstat -ano

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\NETSTAT.EXE` |
| Original File Name | `netstat.exe` |
| Command Line | `netstat -ano` |
| Description | `TCP/IP Netstat Command` |
| Parent Process | `C:\Windows\System32\cmd.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `2184` |
| Parent Process ID | `4144` |
| Integrity Level | `Medium` |
| Event Time | `2026-07-25 02:35:11.888 UTC` |

The second execution used the **`-ano`** option, which displays all active connections, listening ports, numerical addresses, and the associated Process IDs (PIDs). The executable, execution path, parent process, and user remained unchanged, indicating another legitimate interactive execution.

> 📸 **Screenshot Required**
>
> **Title:** Netstat -ano Event Details
>
> **Save As:** `screenshots/investigations/netstat/04-netstat-ano-event-details.png`

---

# Timeline

| Time (UTC) | Activity |
|------------|----------|
| 2026-07-25 02:31:07.230 | User executed the `netstat` command. |
| 2026-07-25 02:31:07.241 | Sysmon generated Event ID 1 for the `netstat` execution. |
| 2026-07-25 02:35:11.888 | User executed the `netstat -ano` command. |
| 2026-07-25 02:35:11.904 | Sysmon generated Event ID 1 for the `netstat -ano` execution. |
| 2026-07-25 02:35:11.904 | Both events were stored in the `wazuh-archives` index for investigation. |

---

# 5W Analysis

| Question | Analysis |
|----------|----------|
| **Who** | DESKTOP-8GB0J2A\Oxhun executed the command. |
| **What** | The Windows `netstat.exe` process was created. |
| **When** | 2026-07-25 02:31:07 UTC. |
| **Where** | Windows endpoint **SOC-Win10**. |
| **Why** | To display active network connections, listening ports, routing information, and associated Process IDs (PIDs) for network diagnostics and troubleshooting. |

---

# MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---------|-----------|----|
| Discovery | System Network Connections Discovery | T1049 |

---

# Investigation Findings

The investigation confirmed the following:

- The Windows `netstat.exe` utility was executed successfully.
- Two command variants were observed during the investigation:
  - `netstat`
  - `netstat -ano`
- Both executions used the legitimate Microsoft Windows `netstat.exe` binary.
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

The Windows **netstat.exe** utility is a legitimate command-line tool used to display active network connections, listening ports, routing information, and network statistics.

During this investigation, both `netstat` and `netstat -ano` were executed and analyzed. The telemetry collected by Sysmon confirmed that both commands originated from the trusted Windows System32 directory and were launched interactively through **Command Prompt (`cmd.exe`)** by the logged-on user.

The `netstat` command displays active network connections and listening ports, while `netstat -ano` additionally displays all active connections, numerical addresses, listening ports, and the associated Process IDs (PIDs). These commands are commonly used by system administrators, network engineers, SOC analysts, incident responders, and IT support personnel during network troubleshooting and security investigations.

Although these commands are associated with **MITRE ATT&CK T1049 – System Network Connections Discovery**, no suspicious behavior was observed in this investigation.

The current Wazuh configuration records both executions in the **wazuh-archives** index through Sysmon Event ID 1. Since no Wazuh detection rule matches these activities, no alerts are generated in the **wazuh-alerts** index.

---

# Lab Validation

To validate the behavior of the `netstat` command within the lab environment, multiple execution contexts were tested.

The following scenarios were performed:

- Command Prompt (Standard User)
- Command Prompt (Administrator)
- Windows PowerShell (Standard User)
- Windows PowerShell (Administrator)

In each environment, both `netstat` and `netstat -ano` were executed.

All executions generated Sysmon Process Create (Event ID 1) events, which were successfully indexed in **wazuh-archives**. No corresponding events were generated in **wazuh-alerts**, confirming that the current Wazuh ruleset treats these executions as normal administrative activity.

---

# Conclusion

The investigation determined that both `netstat` and `netstat -ano` executions represent **normal Windows administrative activity**.

The observed processes were executed from the legitimate Windows System32 directory by the logged-on user through Command Prompt. The process metadata, execution context, parent-child relationship, and command-line arguments were consistent with expected system administration, network monitoring, and diagnostic tasks.

Based on the available evidence, the activity is classified as **Benign**, and no further investigation or escalation is required.

---

# Next Investigation

**07-Investigation-Tasklist.md**

The next investigation analyzes the execution of the Windows `tasklist` command and its corresponding Sysmon Process Create (Event ID 1) telemetry collected by Wazuh.

---

# Document Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | July 2026 | Rajeev Kumar | Initial release of the Netstat Process Investigation covering both `netstat` and `netstat -ano` command executions. |