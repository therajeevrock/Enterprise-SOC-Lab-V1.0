# Windows Command Reference

This document provides a quick reference to the Windows commands used throughout the Enterprise SOC Lab investigations.

Each command includes a brief description of its purpose, the investigation category, and the related investigation document where the command is analyzed from a SOC analyst's perspective.

---

## Investigation Whoami

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `whoami` | Displays the currently logged-on user account. | User Enumeration | Investigation-01-Whoami.md |

---

## Investigation Hostname

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `hostname` | Displays the name of the local computer (hostname). | Host Enumeration | Investigation-02-Hostname.md |

---

## Investigation Systeminfo

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `systeminfo` | Displays detailed information about the local computer, operating system, hardware, installed updates, network configuration, and system environment. | System Information Discovery | Investigation-03-Systeminfo.md |

---

## Investigation Ipconfig

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `ipconfig` | Displays the current TCP/IP network configuration of the local computer, including IP addresses, subnet masks, default gateways, and network adapter information. | System Network Configuration Discovery | Investigation-04-Ipconfig.md |

---

## Investigation ARP

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `arp` | Displays and modifies the Address Resolution Protocol (ARP) cache used to map IP addresses to MAC addresses. | System Network Configuration Discovery | Investigation-05-Arp.md |
| `arp -a` | Displays all ARP cache entries for every network interface on the local computer. | System Network Configuration Discovery | Investigation-05-Arp.md |

---

## Investigation Netstat

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `netstat` | Displays active network connections, listening ports, routing tables, and network statistics. | System Network Connections Discovery | Investigation-06-Netstat.md |
| `netstat -ano` | Displays all active connections and listening ports along with the associated Process ID (PID). | System Network Connections Discovery | Investigation-06-Netstat.md |

---

## Investigation Tasklist

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `tasklist` | Displays all currently running processes and their associated Process IDs (PIDs). | Process Discovery | Investigation-07-Tasklist.md |

---

## Investigation Route

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `route` | Displays usage information for the Windows Route utility and supports viewing or modifying the IP routing table. | System Network Configuration Discovery | Investigation-08-Route.md |
| `route print` | Displays the complete IPv4 and IPv6 routing tables of the local system. | System Network Configuration Discovery | Investigation-08-Route.md |

---


## Investigation Net User

| Command | Purpose | Category | Investigation |
|----------|---------|----------|---------------|
| `net user` | Displays all local user accounts on the system. | Account Discovery | Investigation-08-Net-User.md |
| `net user <username>` | Displays detailed information about a specific local user account. | Account Discovery | Investigation-09-Net-User.md |

---