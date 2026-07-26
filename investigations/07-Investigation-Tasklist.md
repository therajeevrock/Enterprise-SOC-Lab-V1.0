# Investigation-07 - Tasklist Process Investigation


# Investigation Information

| Property | Details |
|----------|---------|
| **Investigation ID** | 07 |
| **Investigation Name** | Tasklist Process Investigation |
| **Category** | Process Discovery |
| **Event Category** | Process Creation |
| **Platform** | Windows 10 |
| **Event Source** | Microsoft-Windows-Sysmon |
| **Event ID** | 1 (Process Create) |
| **MITRE ATT&CK** | T1057 - Process Discovery |
| **Difficulty** | Beginner |


# Related References

The following reference documents provide additional information related to this investigation.

| Document | Purpose |
|----------|---------|
| `commands/Windows-Command-Reference.md` | Windows command reference used throughout the Enterprise SOC Lab investigations. |
| `commands/Wazuh-Search-Queries.md` | Wazuh Threat Hunting search queries used to locate investigation events. |

---

# Objective

The objective of this investigation is to analyze the execution of the Windows **tasklist** command using telemetry collected by Sysmon and Wazuh.

The investigation focuses on verifying the process execution, identifying the executing user and parent process, and determining whether the activity represents normal administrative behavior or suspicious process discovery.

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

The Windows **tasklist** command was executed on the Windows 10 endpoint to generate a Sysmon Process Create (Event ID 1), allowing the activity to be collected and analyzed in Wazuh.

Commands Executed:

```cmd
tasklist
```

Observed Output:

```text
PS C:\Users\Oxhun> tasklist


Image Name                     PID Session Name        Session#    Mem Usage
========================= ======== ================ =========== ============
System Idle Process              0 Services                   0          8 K
System                           4 Services                   0          N/A
Registry                        72 Services                   0      8,656 K
smss.exe                       536 Services                   0          N/A
csrss.exe                      640 Services                   0      1,128 K
wininit.exe                    748 Services                   0        564 K
csrss.exe                      756 Console                    1      1,772 K
winlogon.exe                   808 Console                    1      1,224 K
services.exe                   872 Services                   0      3,400 K
WerFault.exe                   880 Services                   0     13,924 K
lsass.exe                      892 Services                   0      8,112 K
fontdrvhost.exe                980 Services                   0         32 K
fontdrvhost.exe                988 Console                    1      1,416 K
svchost.exe                    512 Services                   0     10,788 K
svchost.exe                    740 Services                   0      8,128 K
dwm.exe                       1072 Console                    1     51,840 K
svchost.exe                   1172 Services                   0     35,356 K
svchost.exe                   1180 Services                   0      2,856 K
svchost.exe                   1196 Services                   0     13,552 K
svchost.exe                   1212 Services                   0     14,580 K
svchost.exe                   1268 Services                   0      5,152 K
svchost.exe                   1396 Services                   0      8,680 K
svchost.exe                   1620 Services                   0      5,880 K
svchost.exe                   1628 Services                   0      1,396 K
svchost.exe                   1660 Services                   0      1,240 K
Memory Compression            1920 Services                   0   1,43,404 K
svchost.exe                   2000 Services                   0      1,892 K
svchost.exe                      8 Services                   0        260 K
svchost.exe                   1500 Services                   0      9,004 K
svchost.exe                   1496 Services                   0      1,000 K
svchost.exe                   1908 Services                   0      1,292 K
spoolsv.exe                   2196 Services                   0      1,812 K
svchost.exe                   2252 Services                   0      3,044 K
svchost.exe                   2560 Services                   0      1,224 K
svchost.exe                   2568 Services                   0     11,200 K
MpDefenderCoreService.exe     2700 Services                   0      2,480 K
snmp.exe                      2788 Services                   0      1,852 K
Sysmon64.exe                  2804 Services                   0      5,244 K
VGAuthService.exe             2860 Services                   0        332 K
vm3dservice.exe               2896 Services                   0        868 K
svchost.exe                   2912 Services                   0        988 K
vmtoolsd.exe                  2920 Services                   0      5,644 K
wazuh-agent.exe               2988 Services                   0     12,888 K
svchost.exe                   3060 Services                   0      5,972 K
MsMpEng.exe                   3068 Services                   0   1,11,476 K
vm3dservice.exe               3228 Console                    1      1,228 K
SearchIndexer.exe             3380 Services                   0     16,256 K
dllhost.exe                   3648 Services                   0      2,036 K
dllhost.exe                   3944 Services                   0      2,304 K
dllhost.exe                   3976 Services                   0      1,292 K
unsecapp.exe                  4336 Services                   0      1,380 K
WmiPrvSE.exe                  4352 Services                   0      8,052 K
TrustedInstaller.exe          4564 Services                   0      1,364 K
AggregatorHost.exe            4636 Services                   0      2,396 K
WmiPrvSE.exe                  4684 Services                   0      4,068 K
SearchProtocolHost.exe        4748 Services                   0      9,168 K
TiWorker.exe                  4816 Services                   0      1,692 K
sihost.exe                    5004 Console                    1     12,336 K
svchost.exe                   5040 Console                    1     22,816 K
taskhostw.exe                 4016 Console                    1      6,124 K
msdtc.exe                     4660 Services                   0      1,100 K
svchost.exe                   3644 Services                   0        156 K
explorer.exe                  2464 Console                    1     79,452 K
SearchFilterHost.exe          4184 Services                   0      2,980 K
svchost.exe                   4784 Services                   0      2,604 K
VSSVC.exe                     1060 Services                   0        592 K
WmiApSrv.exe                  5568 Services                   0        776 K
svchost.exe                   5744 Console                    1      8,196 K
NisSrv.exe                    5948 Services                   0      1,612 K
StartMenuExperienceHost.e     2632 Console                    1     16,476 K
RuntimeBroker.exe             6332 Console                    1      6,896 K
UserOOBEBroker.exe            6520 Console                    1      1,052 K
ctfmon.exe                    6580 Console                    1     13,548 K
TabTip.exe                    6596 Console                    1      2,916 K
SearchApp.exe                 6692 Console                    1   1,90,876 K
RuntimeBroker.exe             6796 Console                    1     21,744 K
backgroundTaskHost.exe        6340 Console                    1      2,544 K
backgroundTaskHost.exe        5836 Console                    1      3,144 K
backgroundTaskHost.exe        6528 Console                    1      6,576 K
backgroundTaskHost.exe        6044 Console                    1      2,936 K
RuntimeBroker.exe             5656 Console                    1     16,104 K
RuntimeBroker.exe             3160 Console                    1      1,156 K
backgroundTaskHost.exe         968 Console                    1      6,800 K
HxTsr.exe                     5232 Console                    1     12,888 K
smartscreen.exe               5844 Console                    1      9,208 K
SecurityHealthSystray.exe     1208 Console                    1      2,044 K
SecurityHealthService.exe     1668 Services                   0      3,396 K
vmtoolsd.exe                  2496 Console                    1      5,796 K
OneDrive.exe                  5684 Console                    1     61,776 K
RuntimeBroker.exe             7404 Console                    1      5,188 K
OneDrive.Sync.Service.exe     7508 Console                    1     75,836 K
conhost.exe                   7548 Services                   0      8,040 K
Greenshot.exe                 7568 Console                    1     45,940 K
TextInputHost.exe             7752 Console                    1     36,588 K
m365copilot_autostarter.e     7880 Console                    1     10,296 K
MicrosoftEdgeUpdate.exe       5524 Services                   0      9,028 K
dllhost.exe                   7444 Console                    1     12,132 K
powershell.exe                6880 Services                   0      2,068 K
conhost.exe                   1764 Services                   0      9,852 K
cmd.exe                       7800 Console                    1      4,024 K
conhost.exe                   6956 Console                    1     24,384 K
tasklist.exe                  5972 Console                    1      9,212 K
```

> 📸 **Screenshot Required**
>
> **Title:** Tasklist Command Execution
>
> **Save As:** `screenshots/investigations/tasklist/01-command.png`

---

# Event Information

> **Note**
>
> The execution of the Windows `tasklist` command does not generate a Wazuh alert because no detection rule matches this activity.
>
> The process creation event is collected by Sysmon (Event ID 1) and stored in the **wazuh-archives** index, where it can be used for threat hunting and forensic investigations.

| Property | Value |
|----------|-------|
| Event Type | Process Create |
| Event ID | 1 |
| Severity | Informational |
| Process | tasklist.exe |
| User | DESKTOP-8GB0J2A\Oxhun |
| Computer | DESKTOP-8GB0J2A |
| Event Time (UTC) | 2026-07-25 05:13:16.740 |

> 📸 **Screenshot Required**
>
> **Title:** Tasklist Event in Wazuh Discover
>
> **Save As:** `screenshots/investigations/tasklist/02-wazuh-event.png`

---

# Event Summary

The investigation analyzed two executions of the Windows **tasklist.exe** utility on the monitored endpoint.

The first execution was launched from **Command Prompt (cmd.exe)**, while the second execution was launched from **Windows PowerShell (powershell.exe)**.

Both executions used the same legitimate Microsoft **tasklist.exe** binary to enumerate the currently running processes on the endpoint.

Sysmon successfully recorded both executions as **Event ID 1 (Process Create)**, and the events were stored in the **wazuh-archives** index for threat hunting and forensic analysis. No corresponding Wazuh alerts were generated.

---

# Event Analysis

## Execution 1 – tasklist (Command Prompt)

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\tasklist.exe` |
| Original File Name | `tasklist.exe` |
| Command Line | `tasklist` |
| Description | `Lists the current running tasks` |
| Parent Process | `C:\Windows\System32\cmd.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `1528` |
| Parent Process ID | `4144` |
| Integrity Level | `Medium` |
| Event Time | `2026-07-25 05:13:16.740 UTC` |

The first execution launched the legitimate Microsoft **tasklist.exe** utility from **Command Prompt**. The command enumerated all running processes on the endpoint and generated a Sysmon Event ID 1.

> 📸 **Screenshot Required**
>
> **Title:** Tasklist Event Details (CMD)
>
> **Save As:** `screenshots/investigations/tasklist/03-tasklist-cmd-event-details.png`

---

## Execution 2 – tasklist (Windows PowerShell)

| Field | Value |
|--------|-------|
| Image | `C:\Windows\System32\tasklist.exe` |
| Original File Name | `tasklist.exe` |
| Command Line | `"C:\Windows\System32\tasklist.exe"` |
| Description | `Lists the current running tasks` |
| Parent Process | `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe` |
| User | `DESKTOP-8GB0J2A\Oxhun` |
| Process ID | `1884` |
| Parent Process ID | `7100` |
| Integrity Level | `Medium` |
| Event Time | `2026-07-25 05:20:16.867 UTC` |

The second execution launched the same **tasklist.exe** utility from **Windows PowerShell**. Although the parent process differed, the executable, execution path, user, and integrity level remained consistent with legitimate administrative activity.

> 📸 **Screenshot Required**
>
> **Title:** Tasklist Event Details (PowerShell)
>
> **Save As:** `screenshots/investigations/tasklist/04-tasklist-powershell-event-details.png`

---

# Timeline

| Time (UTC) | Activity |
|------------|----------|
| 2026-07-25 05:13:16.740 | User executed the `tasklist` command from Command Prompt. |
| 2026-07-25 05:13:16.807 | Sysmon generated Event ID 1 for the Command Prompt execution. |
| 2026-07-25 05:20:16.867 | User executed the `tasklist` command from Windows PowerShell. |
| 2026-07-25 05:20:16.908 | Sysmon generated Event ID 1 for the PowerShell execution. |
| 2026-07-25 05:20:16.908 | Both events were stored in the `wazuh-archives` index for investigation. |

---

# 5W Analysis

| Question | Analysis |
|----------|----------|
| **Who** | DESKTOP-8GB0J2A\Oxhun executed the command. |
| **What** | The Windows `tasklist.exe` process was created. |
| **When** | 2026-07-25 05:13:16 UTC and 2026-07-25 05:20:16 UTC. |
| **Where** | Windows endpoint **SOC-Win10**. |
| **Why** | To enumerate the currently running processes and their Process IDs (PIDs) for system administration, troubleshooting, and security analysis. |

---

# MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---------|-----------|----|
| Discovery | Process Discovery | T1057 |

---

# Investigation Findings

The investigation confirmed the following:

- The Windows `tasklist.exe` utility was executed successfully.
- Two executions of the `tasklist` command were observed during the investigation.
- Both executions used the legitimate Microsoft Windows `tasklist.exe` binary.
- The executable was launched from the trusted `C:\Windows\System32\` directory.
- Both processes were initiated by the logged-on user `DESKTOP-8GB0J2A\Oxhun`.
- One execution was launched from **Command Prompt (`cmd.exe`)**, while the other was launched from **Windows PowerShell (`powershell.exe`)**.
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

The Windows **tasklist.exe** utility is a legitimate command-line tool used to display all currently running processes and their associated Process IDs (PIDs).

During this investigation, the `tasklist` command was executed from both **Command Prompt** and **Windows PowerShell**. The telemetry collected by Sysmon confirmed that both executions originated from the trusted Windows System32 directory and were performed by the logged-on user.

Although the parent processes differed (`cmd.exe` and `powershell.exe`), both executions launched the same legitimate Microsoft `tasklist.exe` binary. This demonstrates that the same executable can have different parent processes depending on the shell used to execute the command.

The `tasklist` command is commonly used by system administrators, SOC analysts, incident responders, malware analysts, and IT support personnel to enumerate running processes during system administration, troubleshooting, and security investigations.

Although this command is associated with **MITRE ATT&CK T1057 – Process Discovery**, no suspicious behavior was observed during this investigation.

The current Wazuh configuration records these executions in the **wazuh-archives** index through Sysmon Event ID 1. Since no Wazuh detection rule matches this activity, no alerts are generated in the **wazuh-alerts** index.

---

# Lab Validation

To validate the behavior of the `tasklist` command within the lab environment, multiple execution contexts were tested.

The following scenarios were performed:

- Command Prompt (Standard User)
- Command Prompt (Administrator)
- Windows PowerShell (Standard User)
- Windows PowerShell (Administrator)

In each environment, the `tasklist` command was executed.

All executions generated Sysmon Process Create (Event ID 1) events, which were successfully indexed in the **wazuh-archives** index. No corresponding events were generated in the **wazuh-alerts** index, confirming that the current Wazuh ruleset treats this activity as normal administrative behavior.

---

# Conclusion

The investigation determined that the execution of the Windows `tasklist` command represents **normal Windows administrative activity**.

The observed processes were executed from the legitimate Windows System32 directory by the logged-on user through both **Command Prompt** and **Windows PowerShell**. The process metadata, execution context, parent-child relationships, and command-line arguments were consistent with expected administrative and troubleshooting activities.

Based on the available evidence, the activity is classified as **Benign**, and no further investigation or escalation is required.

---

# Next Investigation

**08-Investigation-Net-User.md**

The next investigation analyzes the execution of the Windows `net user` command and its corresponding Sysmon Process Create (Event ID 1) telemetry collected by Wazuh.

---

# Document Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | July 2026 | Rajeev Kumar | Initial release of the Tasklist Process Investigation covering command executions from both Command Prompt and Windows PowerShell. |