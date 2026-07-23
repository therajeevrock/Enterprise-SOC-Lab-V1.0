# Investigation Guide

---

# Chapter Information

| Property | Details |
|----------|---------|
| **Chapter Number** | 18 |
| **Difficulty** | Beginner |
| **Estimated Reading Time** | 5–10 Minutes |
| **Estimated Lab Time** | N/A |
| **Category** | Investigation Guide |
| **Next Chapter** | Investigation-01-Whoami |

---

# Document Information

| Property | Details |
|----------|---------|
| **Project** | Enterprise SOC Lab |
| **Document** | Investigation Guide |
| **Version** | 1.0 |
| **Author** | Rajeev Rock |
| **Category** | Investigation Guide |
| **Last Updated** | July 2026 |

---

# Purpose

This guide defines the standard investigation workflow used throughout the Enterprise SOC Lab.

Each investigation follows the same methodology to ensure consistency, improve documentation, and simulate the workflow of a SOC Analyst during endpoint investigations.

---

# Investigation Workflow

Every investigation in this repository follows these steps:

1. Execute the command.
2. Review the command output.
3. Identify useful security information.
4. Check related Windows artifacts.
5. Review Wazuh alerts (if generated).
6. Record the investigation findings.

---

# Standard Investigation Template

Every investigation document includes the following sections.

| Section | Purpose |
|---------|---------|
| Objective | Investigation goal |
| Command | Command executed |
| Expected Output | Expected system output |
| Investigation | Security analysis |
| Wazuh Visibility | Related alerts or logs |
| MITRE ATT&CK | Technique mapping (if applicable) |
| Evidence | Screenshots or command output |
| Conclusion | Final findings |

---

# Investigation Environment

All investigations are performed using the following lab environment.

| Component | Role |
|----------|------|
| Windows 11 Host | SOC Analyst Workstation |
| Ubuntu Server | Wazuh Platform |
| Windows 10 | Monitored Endpoint |
| Kali Linux | Attacker Machine |

---

# Investigation Rules

During every investigation:

- Execute commands only on the intended endpoint.
- Record the exact command that was executed.
- Preserve the original command output whenever possible.
- Verify whether the activity appears in the Wazuh Dashboard.
- Capture screenshots of important findings.
- Document any unexpected behavior.

---

# Evidence Collection

The following evidence should be collected whenever applicable.

- Command executed
- Command output
- Wazuh alert
- Event details
- Timestamp
- Screenshot

---

# Investigation Categories

The investigations in this project cover multiple Windows administration and security areas.

| Category | Examples |
|----------|----------|
| User Enumeration | `whoami`, `net user` |
| System Information | `hostname`, `systeminfo`, `Get-ComputerInfo` |
| Network | `ipconfig`, `arp`, `route`, `netstat`, `nslookup`, `ping` |
| Services | `sc query`, `net start`, `Get-Service` |
| Processes | `tasklist`, `Get-Process` |
| Drivers | `driverquery` |
| File Operations | `mkdir`, `copy`, `move`, `del` |
| Registry | `reg query` |
| PowerShell | `Get-NetIPAddress` |

---

# Investigation Goals

The purpose of these investigations is to:

- Understand Windows administrative commands.
- Learn what information each command reveals.
- Identify the security value of the output.
- Observe endpoint activity through Wazuh.
- Build practical SOC investigation skills.
- Develop a structured SOC investigation methodology.

---

# Prerequisites

Before starting the investigations, ensure that:

- Ubuntu Server is running.
- Wazuh Dashboard is accessible.
- Windows 10 endpoint is online.
- Wazuh Agent is connected.
- Sysmon is installed (where required).
- The endpoint status is **Active** in Wazuh.

---

Each investigation in this repository builds upon the methodology introduced in this guide. Following a consistent investigation process helps develop practical SOC analyst skills and improves documentation quality.

# Next Investigation

Begin with:

`Investigation-01-Whoami.md`