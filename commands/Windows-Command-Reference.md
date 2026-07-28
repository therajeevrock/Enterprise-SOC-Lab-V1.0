# Windows Command Reference

This document provides a quick reference to the Windows commands used throughout the Enterprise SOC Lab investigations.

Each command includes a brief description of its purpose, the investigation category, and the related investigation document where the command is analyzed from a SOC analyst's perspective.

---

## Investigation Whoami

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `whoami` | Displays the currently logged-on user account. | User Enumeration | 01-Investigation-Whoami.md |

---

## Investigation Hostname

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `hostname` | Displays the name of the local computer (hostname). | Host Enumeration | 02-Investigation-Hostname.md |

---

## Investigation Systeminfo

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `systeminfo` | Displays detailed information about the local computer, operating system, hardware, installed updates, network configuration, and system environment. | System Information Discovery | 03-Investigation-Systeminfo.md |

---

## Investigation Ipconfig

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `ipconfig` | Displays the current TCP/IP network configuration of the local computer, including IP addresses, subnet masks, default gateways, and network adapter information. | System Network Configuration Discovery | 04-Investigation-Ipconfig.md |

---

## Investigation ARP

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `arp` | Displays and modifies the Address Resolution Protocol (ARP) cache used to map IP addresses to MAC addresses. | System Network Configuration Discovery | 05-Investigation-Arp.md |
| `arp -a` | Displays all ARP cache entries for every network interface on the local computer. | System Network Configuration Discovery | 05-Investigation-Arp.md |

---

## Investigation Netstat

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `netstat` | Displays active network connections, listening ports, routing tables, and network statistics. | System Network Connections Discovery | 06-Investigation-Netstat.md |
| `netstat -ano` | Displays all active connections and listening ports along with the associated Process ID (PID). | System Network Connections Discovery | 06-Investigation-Netstat.md |

---

## Investigation Tasklist

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `tasklist` | Displays all currently running processes and their associated Process IDs (PIDs). | Process Discovery | 07-Investigation-Tasklist.md |

---

## Investigation Route

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `route` | Displays usage information for the Windows Route utility and supports viewing or modifying the IP routing table. | System Network Configuration Discovery | 08-Investigation-Route.md |
| `route print` | Displays the complete IPv4 and IPv6 routing tables of the local system. | System Network Configuration Discovery | 08-Investigation-Route.md |

---


## Investigation Net User

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `net user` | Displays all local user accounts on the system. | Account Discovery | 09-Investigation-Net-User.md |
| `net user <username>` | Displays detailed information about a specific local user account. | Account Discovery | 09-Investigation-Net-User.md |

---

## Investigation Net LocalGroup

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `net localgroup` | Displays all local security groups configured on the Windows system. | Permission Groups Discovery | 10-Investigation-Net-LocalGroup.md |
| `net localgroup <groupname>` | Displays the members of a specific local security group. | Permission Groups Discovery | 10-Investigation-Net-LocalGroup.md |

---

## Investigation NSLookup

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `nslookup` | Queries DNS servers to resolve domain names into IP addresses or perform reverse DNS lookups for network troubleshooting. | System Network Configuration Discovery | 11-Investigation-NSLookup.md |
| `nslookup <hostname>` | Resolves a specific hostname or domain name to its corresponding IP address using DNS. | System Network Configuration Discovery | 11-Investigation-NSLookup.md |

---

## Investigation Net View

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `net view` | Displays a list of computers in the current domain or workgroup. | Network Share Discovery | 12-Investigation-Net-View.md |
| `net view \\<ComputerName>` | Displays the shared resources available on a specified remote computer. | Network Share Discovery | 12-Investigation-Net-View.md |

---

## Investigation Net Use

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `net use` | Displays active network connections and mapped drives. It is also used to connect, disconnect, and manage shared network resources. | Network Share Connection Discovery | 14-Investigation-Net-Use.md |
| | `net use \\<ComputerName>\<ShareName>` | Connects to a remote shared resource using the specified computer and share name. | Network Share Connection Discovery | 14-Investigation-Net-Use.md |

---

## Investigation Net Session

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `net session` | Displays active sessions established between the local computer and remote clients. | Session Discovery | 15-Investigation-Net-Session.md |
| `net session \\<ComputerName>` | Displays session information for a specified remote computer. | Session Discovery | 15-Investigation-Net-Session.md |

---

## Investigation Net Start

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `net start` | Displays all currently running Windows services. It can also be used to start a specific service. | Service Discovery | 16-Investigation-Net-Start.md |
| `net start <ServiceName>` | Starts the specified Windows service if it is not already running. | Service Discovery | 16-Investigation-Net-Start.md |

---

## Investigation Net Accounts

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `net accounts` | Displays the current password and logon policy configured on the local computer or domain. | Account Policy Discovery | 17-Investigation-Net-Accounts.md |
| `net accounts /domain` | Displays the password and logon policy for the current domain. | Account Policy Discovery | 17-Investigation-Net-Accounts.md |

---

## Investigation Net Config Workstation

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `net config workstation` | Displays configuration information for the Workstation service, including computer name, logged-on user, and workstation settings. | System Configuration Discovery | 18-Investigation-Net-Config-Workstation.md |

---

## Investigation Net Config Server

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `net config server` | Displays configuration information for the Server service, including server settings and service parameters. | System Configuration Discovery | 19-Investigation-Net-Config-Server.md |