# Prerequisites

---

## Chapter Information

| Property                   | Details |
|----------------------------|---------|
| **Chapter Number**         | 02 |
| **Difficulty**             | Beginner |
| **Estimated Reading Time** | 15–20 Minutes |
| **Estimated Lab Time**     |  N/A (Planning Chapter) |
| **Prerequisite**           | 01 - Lab Architecture |
| **Learning Outcome**       | Understand the hardware, software, operating systems, virtualization platform, and networking requirements needed before building the Enterprise SOC Lab. |
| **Next Chapter**           | 03 - Virtual Lab Setup |

---

## Document Information

| Property          | Details            |
|-------------------|--------------------|
| **Project**       | Enterprise SOC Lab |
| **Document Name** | Prerequisites      |
| **Version**       | 1.0                |
| **Author**        | Rajeev Rock        |
| **Category**      | Documentation      |
| **Status**        | Published          |
| **Last Updated**  | July 2026          |

---

## Learning Objectives

After completing this chapter, you will be able to:

- Identify the minimum and recommended hardware requirements for the SOC lab.
- Understand the software required to build the Enterprise SOC Lab.
- Explain the purpose of each operating system used throughout the project.
- Understand why virtualization is required for building a realistic SOC environment.
- Identify the required ISO images, software packages, and installation files.
- Understand the networking requirements before deploying the virtual machines.
- Verify that the environment is fully prepared before beginning the installation process.

---

## Objective

Before building an Enterprise Security Operations Center (SOC) Lab, it is important to prepare the required hardware, software, operating systems, and virtualization environment.

Many installation problems occur because users begin installing security tools without verifying whether their systems meet the required specifications.

The objective of this chapter is to ensure that every prerequisite is available before starting the lab setup.

After completing this chapter, the reader will understand:

- The hardware required to build the lab.
- The software components used throughout the project.
- The operating systems and their responsibilities.
- The virtualization platform used to host the environment.
- The ISO images and installation files that must be downloaded.
- The networking requirements for communication between virtual machines.
- The recommended system resources for running the lab efficiently.
- How to verify that the environment is ready before beginning the installation process.

Preparing the environment correctly reduces installation failures, improves system stability, and provides a solid foundation for the remaining chapters of this project.

---

# Introduction

Building a Security Operations Center (SOC) Lab involves running multiple operating systems and security tools simultaneously.

Unlike a normal desktop application, a SOC lab consists of several independent machines communicating with each other over a virtual network.

In this project, VMware Workstation is used to host multiple virtual machines that simulate an enterprise environment.

Each virtual machine has a dedicated responsibility within the monitoring pipeline.

Before installing Ubuntu Server, Wazuh, Sysmon, or any other security tool, it is essential to verify that the host system has sufficient hardware resources to support all virtual machines.

This chapter explains every requirement in detail and provides recommendations based on practical experience while building this lab.

---

# Software Requirements

In addition to the required hardware, several software components are needed to build and operate the Enterprise SOC Lab successfully.

Each software component has a specific responsibility within the environment. Together, these applications create a complete attack, monitoring, and investigation platform.

The following software was selected based on stability, community support, enterprise adoption, and compatibility with the Wazuh platform.

### Lab Software Components

| Software | Purpose |
| :--- | :--- |
| **VMware Workstation** | Hosts all virtual machines |
| **Windows 11** | Host operating system for the SOC Analyst workstation |
| **Ubuntu Server LTS** | Hosts the Wazuh platform |
| **Windows 10** | Simulates an employee endpoint |
| **Kali Linux** | Simulates the attacker machine |
| **Wazuh Platform** | Centralized Security Information and Event Management (SIEM) platform |
| **Sysmon** | Generates detailed Windows security telemetry |
| **Wazuh Agent** | Collects Windows logs and forwards them to the Wazuh Manager |
| **Web Browser** | Accesses the Wazuh Dashboard |

Every software component used throughout this project will be installed and configured in the upcoming chapters.

---

# Hardware Requirements

## Minimum Requirements

| Component | Minimum Requirement |
|----------|----------------------|
| Processor | 4 Physical Cores (64-bit with Virtualization Support) |
| Memory (RAM) | 8 GB (Limited Lab Experience) |
| Storage | 200 GB SSD |
| Operating System | Windows 10 / Windows 11 (64-bit) |
| Virtualization | Intel VT-x / AMD-V Enabled |
| Internet Connection | Required |

> **Note**
>
>  A system with 8 GB RAM can run this lab, but performance may be limited. You may need to reduce virtual machine resources or avoid running all virtual machines simultaneously.

---

## Lab Configuration Used in This Project

The Enterprise SOC Lab documented in this repository was built and tested using the following hardware configuration.

| Component | Configuration Used |
|----------|--------------------|
| Processor | Intel Core i7 (11th Generation, 4 Cores / 8 Threads) |
| Memory (RAM) | 16 GB |
| Storage | 500 GB SSD |
| Host Operating System | Windows 11 (64-bit) |
| Virtualization Platform | VMware Workstation Pro |

> **Note**
>
> This project was developed and tested on an Intel Core i7-1185G7 system with 16 GB RAM running Windows 11 and VMware Workstation Pro. Similar hardware with equivalent performance should provide a comparable experience.

## Recommended Requirements

| Component | Recommended Requirement |
|----------|--------------------------|
| Processor | 6–8 Physical Cores |
| Memory (RAM) | 32 GB or Higher |
| Storage | 500 GB SSD or NVMe SSD |
| Operating System | Windows 11 (64-bit) |
| Virtualization | Intel VT-x / AMD-V Enabled |
| Internet Connection | Stable Broadband Connection |

---

# Why 16 GB RAM?

A common question among beginners is why this project was built using a system with **16 GB of RAM** when the minimum requirement is **8 GB**.

Although the Enterprise SOC Lab can run on a system with 8 GB of RAM, performance may be limited. Running multiple virtual machines simultaneously can consume a significant amount of memory, causing slower performance and reduced responsiveness.

During this project, the following systems may run at the same time:

- Windows 11 Host Machine
- Ubuntu Server (Wazuh Platform)
- Windows 10 Endpoint
- Kali Linux

In addition to the operating systems, services such as the Wazuh Manager, OpenSearch Indexer, Filebeat, and the Wazuh Dashboard also require system memory.

The Enterprise SOC Lab documented in this repository was built and tested using **16 GB of RAM**, which provided a stable and responsive environment for the lab.

For users planning to expand the lab by adding additional virtual machines or enterprise services, **32 GB or more** is recommended for the best experience.

# Operating Systems Used

This project uses multiple operating systems to simulate the different roles commonly found in an enterprise environment.

Each operating system was selected based on its intended responsibility within the SOC architecture.

## Operating System Overview

| Operating System | Role |
|------------------|------|
| Windows 11 | SOC Analyst Workstation |
| Ubuntu Server | Wazuh Platform |
| Windows 10 | Employee Endpoint |
| Kali Linux | Attacker Machine |

## Windows 11

Windows 11 is used as the host operating system.

It represents the SOC Analyst's workstation where investigations are performed using the Wazuh Dashboard.

Responsibilities include:

- Running VMware Workstation
- Accessing the Wazuh Dashboard
- Investigating alerts
- Threat hunting
- Documentation

---

## Ubuntu Server

Ubuntu Server is used to host the Wazuh platform.

It runs the following services:

- Wazuh Manager
- Wazuh Dashboard
- OpenSearch Indexer
- Filebeat

Ubuntu Server acts as the central monitoring server of the entire SOC lab.

---

## Windows 10

Windows 10 represents an employee workstation.

It generates endpoint telemetry through Sysmon and forwards Windows events using the Wazuh Agent.

This machine is continuously monitored throughout the project.

---

## Kali Linux

Kali Linux represents the attacker.

It is used to simulate real-world attack scenarios including reconnaissance, exploitation, and post-exploitation activities.

Security events generated during these exercises are monitored and investigated using Wazuh.

---

# Virtualization Requirements

Virtualization is the foundation of this Enterprise SOC Lab.

Instead of using multiple physical computers, VMware Workstation is used to create isolated virtual machines that communicate over a virtual network.

Using virtualization provides several advantages:

- Safe experimentation
- Easy recovery using snapshots
- Network isolation
- Efficient hardware utilization
- Enterprise-like infrastructure on a single computer

All virtual machines used in this project are hosted inside VMware Workstation.

---

# Network Requirements

Proper network configuration is essential for communication between the virtual machines.

Throughout this project, VMware networking is used to allow the Windows endpoint, Ubuntu Server, Kali Linux, and the Windows 11 host machine to communicate with each other.

The selected network mode should allow:

- Communication between all virtual machines.
- Communication between the host machine and the Ubuntu Server.
- Internet access for downloading updates and packages.
- Secure communication between the Wazuh Agent and the Wazuh Manager.

Network configuration will be explained in detail during the Virtual Lab Setup chapter.

---

# Required Downloads

Before beginning the installation process, ensure that all required installation media and software packages are available.

Required downloads include:

| Download | Purpose |
|----------|---------|
| VMware Workstation Pro | Virtualization Platform |
| Ubuntu Server ISO | Wazuh Server |
| Windows 10 VMware Image | Employee Endpoint |
| Kali Linux VMware Image | Attacker Machine |
| Sysmon | Windows Monitoring |
| Wazuh Agent | Log Collection |

Downloading these components before starting the installation process helps avoid interruptions during the lab setup.

---

# Lab Preparation Checklist

Before starting the installation, verify that your environment is ready.

- [ ] Windows 11 host machine is operational.
- [ ] Hardware virtualization (Intel VT-x / AMD-V) is enabled in BIOS/UEFI.
- [ ] VMware Workstation is installed successfully.
- [ ] Ubuntu Server ISO has been downloaded.
- [ ] Windows 10 VMware Image has been downloaded.
- [ ] Kali Linux VMware Image has been downloaded.
- [ ] At least 200 GB of free SSD storage is available.
- [ ] Sufficient RAM is available for running multiple virtual machines.
- [ ] A stable internet connection is available.
- [ ] All required installation files have been downloaded and verified.

Completing this checklist before starting the installation process helps prevent common setup issues and provides a stable foundation for the Enterprise SOC Lab.

---

# Summary

This chapter introduced all prerequisites required to build the Enterprise SOC Lab.

We discussed the minimum and recommended hardware requirements, software components, operating systems, virtualization platform, networking requirements, and the resources needed before beginning the installation process.

Preparing the environment correctly helps reduce installation issues and provides a stable foundation for the remaining chapters.

---

# Key Takeaways

After completing this chapter, the reader should understand:

- The hardware required for the lab.
- The software used throughout the project.
- The purpose of each operating system.
- Why virtualization is required.
- Basic networking requirements.
- The resources that should be prepared before installation begins.

These prerequisites ensure that the environment is ready for deploying the complete SOC infrastructure.

---

# Next Chapter

The next chapter focuses on designing and preparing the virtual infrastructure for the Enterprise SOC Lab.

Topics include:

- VMware Workstation installation
- Virtual machine planning
- Resource allocation
- Virtual networking
- Folder organization
- Snapshot strategy
- Best practices

**Next Document:** `03-Virtual-Lab-Setup.md`

# Document Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | July 2026 | Rajeev Rock | Initial release of the Prerequisites documentation for the Enterprise SOC Lab. |