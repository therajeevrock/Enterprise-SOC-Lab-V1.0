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