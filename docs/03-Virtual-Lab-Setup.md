# Virtual Lab Setup

---

## Chapter Information

| Property                   | Details |
|----------------------------|---------|
| **Chapter Number**         | 03 |
| **Difficulty**             | Beginner |
| **Estimated Reading Time** | 25–35 Minutes |
| **Estimated Lab Time**     | 45–60 Minutes |
| **Prerequisite**           | 02 - Prerequisites |
| **Learning Outcome**       | Understand how to design, organize, and prepare the virtual infrastructure required for the Enterprise SOC Lab, including VMware Workstation, virtual machines, networking, and resource allocation. |
| **Next Chapter**           | 04 - Ubuntu Server Installation |

---

## Document Information

| Property          | Details |
|-------------------|--------------------|
| **Project**       | Enterprise SOC Lab |
| **Document Name** | Virtual Lab Setup |
| **Version**       | 1.0 |
| **Author**        | Rajeev Rock |
| **Category**      | Documentation |
| **Status**        | Published |
| **Last Updated**  | July 2026 |

---

## Learning Objectives

After completing this chapter, you will be able to:

- Understand the purpose of virtualization in a Security Operations Center (SOC) lab.
- Explain why VMware Workstation was selected for this project.
- Plan and organize the virtual machines used throughout the lab.
- Allocate CPU, memory, and storage resources efficiently.
- Understand the different VMware network modes and their use cases.
- Configure a virtual network suitable for security monitoring.
- Organize virtual machines, ISO files, and project resources using a structured directory layout.
- Understand the importance of snapshots and recovery strategies.
- Prepare a stable virtual environment before installing the operating systems and security tools.

---

## Objective

The objective of this chapter is to prepare the virtual infrastructure required for building the Enterprise SOC Lab.

Before installing Ubuntu Server, Windows 10, Kali Linux, or the Wazuh platform, it is important to design a well-structured virtual environment. Proper planning of virtual machines, resource allocation, storage organization, and networking helps create a stable and scalable lab that closely resembles an enterprise Security Operations Center (SOC).

---

# Introduction

A Security Operations Center (SOC) relies on multiple systems working together to monitor, collect, analyze, and investigate security events.

In a production environment, these systems are usually deployed on dedicated physical servers, employee workstations, and enterprise networks. Building such an infrastructure in a home environment would require significant hardware resources and cost.

Virtualization provides an efficient solution by allowing multiple operating systems to run simultaneously on a single physical computer. 

In this project, VMware Workstation is used to create a virtual enterprise environment consisting of four separate systems:

- Windows 11 Host (SOC Analyst Workstation)
- Ubuntu Server (Wazuh Platform)
- Windows 10 Endpoint (Employee Machine)
- Kali Linux (Attacker Machine)

Each virtual machine performs a dedicated role within the Enterprise SOC Lab. Separating these responsibilities makes the environment easier to understand, manage, troubleshoot, and expand in future phases of the project.

This chapter focuses on preparing the virtual infrastructure before installing any operating systems or security tools. Proper planning at this stage helps prevent configuration issues and creates a stable foundation for the remaining chapters.

---

# Why Build a Virtual SOC Lab?

Building a Security Operations Center (SOC) using physical computers is expensive, space-consuming, and difficult to maintain.

Virtualization allows multiple isolated systems to run on a single host machine while accurately simulating the architecture of a real enterprise environment.

Using a virtual lab provides several advantages:

- Safe experimentation without affecting the host operating system.
- Complete isolation between attacker and victim machines.
- Easy recovery using virtual machine snapshots.
- Efficient use of hardware resources.
- Flexible network configuration.
- Rapid deployment of new virtual machines.
- Ability to reproduce attack scenarios consistently.
- Practical experience with enterprise security workflows.

For cybersecurity training, virtualization offers the ideal balance between cost, flexibility, and realism. It enables learners to build, modify, break, restore, and investigate systems without risking production environments.

Throughout this project, every attack, configuration change, troubleshooting exercise, and investigation will be performed inside this isolated virtual environment.

---

# Why VMware Workstation Pro?

Several virtualization platforms are available for building cybersecurity labs, including VMware Workstation, Oracle VirtualBox, Microsoft Hyper-V, and Proxmox VE.

For this project, **VMware Workstation Pro** was selected because it provides excellent stability, reliable networking, strong hardware compatibility, and enterprise-grade virtualization features. These capabilities make it well suited for building a realistic Security Operations Center (SOC) lab.

Compared to other virtualization platforms, VMware Workstation offers a smooth experience when running multiple virtual machines simultaneously while maintaining consistent performance.

Key advantages include:

- Stable performance for multiple virtual machines.
- Advanced virtual networking (NAT, Bridged, Host-Only, Custom Networks).
- Snapshot support for quick recovery.
- Broad operating system compatibility.
- Easy resource allocation and management.
- Reliable USB and device passthrough.
- Widely used by cybersecurity professionals, researchers, and training environments.

Another major advantage is the ability to quickly restore virtual machines to a previous state using snapshots. This is particularly valuable during security testing because experiments may intentionally modify or damage operating systems.

Throughout this project, VMware Workstation serves as the virtualization platform that hosts every component of the Enterprise SOC Lab.

---

## Official Download Source

VMware Workstation Pro should always be downloaded from the official Broadcom Support Portal.

| Resource | Link |
|----------|------|
| VMware Workstation Pro | https://support.broadcom.com |

> **Note**
>
> VMware Workstation is distributed through the Broadcom Support Portal. Always download the software from the official source to ensure authenticity, integrity, and access to the latest stable release.

---

## VMware Workstation vs Other Virtualization Platforms

| Platform | Advantages | Limitations |
|----------|------------|-------------|
| **VMware Workstation Pro** | Stable, excellent networking, snapshots, enterprise features | Higher hardware requirements |
| **Oracle VirtualBox** | Free and easy to use | Less consistent performance with multiple VMs |
| **Microsoft Hyper-V** | Built into Windows Pro/Enterprise | Can conflict with other virtualization software and may require additional configuration |
| **Proxmox VE** | Excellent for dedicated server environments | Requires a separate physical server and is more suitable for advanced lab deployments |

For this project, VMware Workstation provides the best balance between performance, ease of use, flexibility, and enterprise-like functionality.

---

## Why VMware Was the Right Choice for This Project

The Enterprise SOC Lab requires multiple operating systems to run simultaneously while maintaining reliable communication between virtual machines.

The selected virtualization platform needed to support:

- Multiple virtual machines running at the same time.
- Flexible virtual networking.
- Stable resource management.
- Easy snapshot creation and recovery.
- Compatibility with Windows, Ubuntu Server, and Kali Linux.
- A smooth learning experience for beginners.

Based on these requirements, VMware Workstation was selected as the primary virtualization platform for this project.

---

# Virtual Machine Planning

Before creating any virtual machines, it is important to define the purpose of each system within the Enterprise SOC Lab.

In a real-world Security Operations Center (SOC), different systems perform different responsibilities. Servers host security platforms, employee workstations generate endpoint activity, analyst workstations investigate security events, and attacker systems simulate malicious behavior.

This lab follows the same architectural approach by assigning a dedicated role to each virtual machine.

Planning the environment before deployment makes the lab easier to manage, troubleshoot, document, and expand in future phases.

---

## Virtual Machines Used in This Lab

| Virtual Machine | Operating System | Primary Role |
|-----------------|------------------|--------------|
| Windows 11 Host | Windows 11 | SOC Analyst Workstation |
| Ubuntu Server | Ubuntu Server LTS | Wazuh Platform |
| Windows 10 VM | Windows 10 | Employee Endpoint |
| Kali Linux VM | Kali Linux | Attacker Machine |

## Virtual Machine Responsibilities

### Windows 11 Host

The Windows 11 host machine represents the SOC Analyst's workstation.

Its responsibilities include:

- Running VMware Workstation.
- Accessing the Wazuh Dashboard.
- Investigating security events.
- Performing threat hunting.
- Documenting findings.
- Managing the virtual lab.

---

### Ubuntu Server

The Ubuntu Server acts as the central monitoring server.

It hosts the core components of the Wazuh platform:

- Wazuh Manager
- Wazuh Dashboard
- OpenSearch Indexer
- Filebeat

Its primary responsibility is to collect, process, store, and visualize security events generated by monitored endpoints.

---

### Windows 10 Endpoint

The Windows 10 virtual machine represents a standard employee workstation.

This machine continuously generates endpoint activity through:

- User logins
- Process execution
- Network communication
- File operations
- Registry modifications
- PowerShell execution

Sysmon records these activities, while the Wazuh Agent forwards the logs to the Ubuntu Server for analysis.

---

### Kali Linux

Kali Linux represents an external attacker.

During later chapters, it will be used to simulate:

- Reconnaissance
- Port scanning
- Service enumeration
- Exploitation
- Reverse shell attacks
- Persistence techniques

Every simulated attack will generate security events that can be detected and investigated using Wazuh.

---

## Enterprise Architecture Mapping

Although this project runs on a single physical computer, it represents the same logical architecture commonly found in enterprise environments.

### Environment Mapping (Enterprise vs. Lab)

#### SOC Analyst Workstation
* **Enterprise Environment:** Dedicated Security Analyst Workstation
* **Home SOC Lab:** Windows 11 Host

#### SIEM Server
* **Enterprise Environment:** Distributed Enterprise SIEM Platform
* **Home SOC Lab:** Ubuntu Server

#### Employee Endpoint
* **Enterprise Environment:** Thousands of Corporate User Endpoints
* **Home SOC Lab:** Windows 10 VM

#### External Attacker
* **Enterprise Environment:** Real-World Threat Actors / Advanced Persistent Threats (APTs)
* **Home SOC Lab:** Kali Linux VM

This design provides practical experience with enterprise security monitoring while remaining simple enough to build and maintain on a personal computer.

---

# Virtual Machine Resource Allocation

Proper resource allocation is one of the most important steps when building a stable virtual environment.

Every virtual machine shares the host computer's CPU, memory, and storage. Allocating too many resources to one machine can slow down the host system, while allocating too few resources can cause services to fail or become unstable.

The goal is to distribute the available hardware efficiently so that all virtual machines can operate simultaneously without significantly affecting the host machine.

---

## Resource Allocation Used in This Project

The following resource allocation was used throughout this project.

| Virtual Machine | vCPU | Memory (RAM) | Storage | Purpose |
|-----------------|:----:|:------------:|:-------:|---------|
| Ubuntu Server | 2 vCPUs | 4 GB | 60 GB | Wazuh Platform |
| Windows 10 VM | 2 vCPUs | 4 GB | 60 GB | Employee Endpoint |
| Kali Linux VM | 3 vCPUs | 6 GB | 80 GB | Attacker Machine |

This configuration provides a stable environment for learning while keeping the host operating system responsive.

---

## Why This Resource Allocation?

Each virtual machine performs a different role and therefore requires different system resources.

### Ubuntu Server

Ubuntu Server hosts multiple services including:

- Wazuh Manager
- OpenSearch Indexer
- Filebeat
- Wazuh Dashboard

Since these services continuously collect, process, index, and display security events, Ubuntu Server receives the largest share of CPU and memory.

---

### Windows 10 Endpoint

The Windows 10 virtual machine represents an employee workstation.

Its primary purpose is to generate endpoint telemetry using Sysmon and forward events through the Wazuh Agent.

Unlike the Wazuh Server, it performs fewer background tasks and therefore requires fewer resources.

---

### Kali Linux

Kali Linux is primarily used for penetration testing and attack simulation.

Most tools are executed manually during practical exercises, so Kali Linux does not require large amounts of memory during normal operation.

Additional resources can be temporarily assigned if future labs require heavier workloads.

---

## Resource Allocation Best Practices

When assigning hardware resources to virtual machines:

- Avoid allocating all available RAM to virtual machines.
- Leave sufficient memory for the host operating system.
- Allocate CPU cores according to the workload of each machine.
- Store virtual machines on an SSD whenever possible.
- Shut down unused virtual machines to free system resources.
- Increase resources only when necessary after monitoring system performance.

Proper resource planning improves stability and provides a better learning experience throughout the project.

---

## Total Resource Allocation

When all virtual machines are configured, the following virtual resources are allocated within the Enterprise SOC Lab.

| Resource | Total Allocation |
|----------|-----------------:|
| Virtual CPUs | 7 vCPUs |
| Memory | 14 GB |
| Virtual Disk Allocation | Approximately 200 GB |

These values represent the **configured virtual resources** assigned to the virtual machines and are **not the actual runtime resource consumption**. The real CPU, memory, and disk usage vary depending on the operating systems, background services, running applications, and attack simulations performed during the practical exercises.

For example, Ubuntu Server may consume more CPU and memory while indexing security events in OpenSearch, whereas Kali Linux typically uses fewer resources until penetration testing tools are executed. Similarly, the Windows 10 endpoint generates varying amounts of telemetry depending on user activity and attack simulations.

Proper resource monitoring throughout the project helps maintain a stable and responsive virtual environment.

> **Note**
>
> The host system used for this project has a **500 GB SSD**. However, only **approximately 200 GB** is allocated to the virtual machines. The remaining storage is reserved for the host operating system, ISO files, snapshots, project documentation, and future lab expansion.

---

# Network Planning

A properly designed network is essential for building a functional Security Operations Center (SOC) lab.

Every virtual machine within the lab must be able to communicate with the systems it is designed to interact with. At the same time, unnecessary exposure should be avoided to maintain a safe and controlled environment.

Before selecting a VMware network mode, it is important to understand how the virtual machines will communicate with each other.

In this project, the network was designed to support the following communication:

- Windows 10 Endpoint communicates with the Ubuntu Server.
- The Wazuh Agent forwards security events to the Wazuh Manager.
- Kali Linux communicates with the Windows 10 Endpoint during attack simulations.
- The Windows 11 host machine accesses the Wazuh Dashboard using a web browser.
- Ubuntu Server accesses the internet for software installation and updates.

The objective is to simulate an enterprise environment where endpoints generate telemetry, attackers perform controlled attacks, and SOC analysts investigate events through a centralized SIEM platform.

---

## Communication Flow

```text
                 Internet
                     │
                     │
          VMware Virtual Network
                     │
      ┌──────────────┼──────────────┐
      │              │              │
      ▼              ▼              ▼

Windows 10      Ubuntu Server     Kali Linux
 Endpoint       Wazuh Platform      Attacker
      │              ▲
      │              │
      └──── Logs ────┘

             ▲
             │ HTTPS
             │
      Windows 11 Host
   SOC Analyst Workstation
```

---

## Communication Requirements

To ensure that the Enterprise SOC Lab functions correctly, the following communication paths must be available.

| Source | Destination | Purpose |
|---------|-------------|---------|
| Windows 10 VM | Ubuntu Server | Forward Sysmon and Windows Event Logs |
| Kali Linux VM | Windows 10 VM | Attack simulation and penetration testing |
| Windows 11 Host | Ubuntu Server | Access the Wazuh Dashboard |
| Ubuntu Server | Internet | Download packages and updates |

If any of these communication paths fail, certain components of the lab may stop functioning correctly. For example, if the Windows endpoint cannot communicate with the Ubuntu Server, security events will not reach the Wazuh platform. Similarly, if the Windows 11 host cannot reach the Ubuntu Server, the Wazuh Dashboard will be inaccessible to the SOC analyst.

Proper network planning is therefore a fundamental requirement before deploying the virtual machines.

---

# VMware Network Modes

VMware Workstation provides multiple networking modes that determine how virtual machines communicate with each other, the host machine, and external networks.

Selecting the correct network mode is essential for building a stable and functional Enterprise SOC Lab.

## Comparison of VMware Network Modes

| Network Mode | Internet Access | VM-to-VM Communication | Host Communication | Typical Use Case |
|--------------|-----------------|------------------------|--------------------|------------------|
| NAT | Yes | Yes | Yes | SOC Labs, Development, Testing |
| Bridged | Yes | Yes | Yes | Enterprise Networks |
| Host-Only | No | Yes | Yes | Isolated Security Labs |

---

## Network Mode Selected for This Project

This project uses **NAT (Network Address Translation)** networking.

NAT provides:

- Internet access for software installation and updates.
- Communication between all virtual machines.
- Communication between the Windows 11 host and Ubuntu Server.
- A secure and isolated lab environment.

NAT offers the best balance between functionality, simplicity, and security for a home SOC lab.

---

# Why NAT Was Selected

Several VMware networking modes were evaluated before building this lab.

NAT was selected because it meets all functional requirements of the Enterprise SOC Lab while remaining simple to configure.

The reasons include:

- Ubuntu Server requires internet access to install Wazuh and system packages.
- Windows 10 must communicate with the Ubuntu Server through the Wazuh Agent.
- Kali Linux must communicate with the Windows 10 endpoint during attack simulations.
- The Windows 11 host must access the Wazuh Dashboard through a web browser.

NAT satisfies all of these requirements without exposing the virtual machines directly to the physical network.

For beginners, NAT also reduces networking complexity while maintaining an enterprise-like deployment experience.

---

# Best Practices

The following practices are recommended when building the virtual lab:

- Allocate resources according to the workload of each virtual machine.
- Store virtual machines on an SSD whenever possible.
- Use snapshots before making major configuration changes.
- Keep ISO files organized in a dedicated folder.
- Verify network connectivity after creating each virtual machine.
- Install and configure one component at a time.
- Test each component before moving to the next chapter.
- Document configuration changes and troubleshooting steps throughout the project.

Following these practices helps create a stable, maintainable, and reproducible Enterprise SOC Lab.

---

# Summary

This chapter introduced the virtual infrastructure used throughout the Enterprise SOC Lab.

We explored the purpose of virtualization, selected VMware Workstation as the virtualization platform, planned the virtual machines, allocated system resources, and designed the network architecture required for security monitoring.

Completing these planning activities before installation provides a stable foundation for the remaining chapters.

---

# Key Takeaways

After completing this chapter, you should be able to:

- Explain why virtualization is required for a SOC lab.
- Describe the role of each virtual machine.
- Allocate resources appropriately for each system.
- Understand VMware networking concepts.
- Explain why NAT networking was selected.
- Prepare the virtual environment before installing operating systems.

---

# Next Chapter

The next chapter focuses on installing and configuring the Ubuntu Server, which will host the Wazuh platform.

Topics include:

- Installing Ubuntu Server
- Initial system configuration
- Network configuration
- System updates
- SSH configuration
- Preparing the server for Wazuh installation

**Next Document:** `04 - Ubuntu Server Installation.md`

---

## Document Revision History

| Version |    Date   |            Changes            |
|---------|-----------|-------------------------------|
|   1.0   | July 2026 | Initial documentation created |