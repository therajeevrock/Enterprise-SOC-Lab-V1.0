# Enterprise SOC Lab Architecture

---

## Chapter Information

| Property                   | Details |
|----------------------------|---------|
| **Chapter Number**         | 01 |
| **Difficulty**             | Beginner |
| **Estimated Reading Time** | 20–30 Minutes |
| **Estimated Lab Time**     | N/A (Theory Chapter) |
| **Prerequisite**           | None |
| **Learning Outcome**       | Understand the complete Enterprise SOC Lab architecture, the role of each virtual machine, the security monitoring pipeline, and how all components work together in a real-world SOC environment. |
| **Next Chapter**           | 02 - Prerequisites |

---

## Document Information

| Property          | Details            |
|-------------------|--------------------|
| **Project**       | Enterprise SOC Lab |
| **Document Name** | Lab Architecture   |
| **Version**       | 1.0                |
| **Author**        | Rajeev Kumar       |
| **Category**      | Documentation      |
| **Status**        | Published          |
| **Last Updated**  | July 2026          |

---

## Learning Objectives

After completing this chapter, you will be able to:

- Explain the overall architecture of the Enterprise SOC Lab.
- Identify the role of each virtual machine.
- Understand how security events travel through the security monitoring pipeline.
- Differentiate between a home SOC lab and an enterprise SOC environment.
- Explain the responsibilities of Windows 11, Ubuntu Server, Windows 10, and Kali Linux within the lab.
- Describe how Sysmon, the Wazuh Agent, the Wazuh Platform, and the Windows endpoint work together.
- Understand how endpoint telemetry reaches the Wazuh Dashboard for investigation.

---

## Objective

The objective of this chapter is to introduce the architecture of the Enterprise SOC Lab and explain the purpose of every major component used throughout the project.

Before deploying operating systems, installing security tools, or performing attack simulations, it is important to understand how the lab is designed and how each virtual machine contributes to the overall security monitoring process.

This chapter provides a high-level overview of the lab environment, including its architecture, virtual machines, monitoring workflow, and security monitoring pipeline.

By the end of this chapter, the reader will have a clear understanding of how the Enterprise SOC Lab is structured and how its components interact with each other to simulate a real-world Security Operations Center (SOC).

---

# Project Objective

The primary objective of this project is to design, build, document, and operate a complete Enterprise Security Operations Center (SOC) Lab in a virtual environment. This lab simulates how modern organizations monitor Windows endpoints, collect security logs, detect suspicious activities, investigate alerts, and respond to potential cyber threats.

Unlike a basic installation guide, this project focuses on understanding the complete security monitoring pipeline from the endpoint to the Security Information and Event Management (SIEM) platform. Every component is configured manually, every issue encountered during the setup is documented, and every troubleshooting step is explained to provide a practical learning experience.

The lab is designed to help beginners understand how enterprise SOC environments actually work while also serving as a professional portfolio project demonstrating hands-on cybersecurity skills.

The project includes:

- Enterprise virtual lab design
- Windows endpoint monitoring
- Wazuh platform deployment
- Sysmon deployment and configuration
- Centralized log collection
- Security event investigation
- Attack simulation
- Threat hunting exercises
- Detection engineering
- MITRE ATT&CK mapping
- Incident response practice
- Professional project documentation

The long-term goal of this project is not only to learn how to use Wazuh, but also to understand how Security Operations Centers operate in real organizations and develop the practical skills required for a SOC Analyst L1 role.

---

# Introduction

Security Operations Centers (SOCs) are responsible for continuously monitoring an organization's infrastructure, detecting malicious activities, investigating security events, and responding to cyber threats before they impact business operations.

Modern organizations generate millions of security events every day. These events originate from employee laptops, servers, Active Directory, firewalls, cloud services, applications, and network devices. Monitoring these events manually is impossible. Therefore, organizations deploy SIEM (Security Information and Event Management) platforms that collect, correlate, and analyze logs from different systems.

This project recreates a simplified enterprise SOC environment using virtual machines. Although the environment contains only a few systems, the monitoring workflow follows the same principles used inside real organizations.

The lab consists of four primary systems:

- Windows 11 Host Machine (SOC Analyst Workstation)
- Ubuntu Server (Wazuh Manager, Indexer, Dashboard)
- Windows 10 Endpoint (Employee Machine)
- Kali Linux (Attacker Machine)

Each system performs a dedicated role within the security monitoring pipeline.

During this project we will not only learn how to install security tools, but also understand why each component exists, how data flows between them, how security events are generated, how logs are collected, and how analysts investigate suspicious activities.

Every major configuration, troubleshooting step, investigation, and lesson learned will be documented throughout this repository.

By the end of this project, the lab will simulate the complete workflow of an enterprise SOC, including endpoint monitoring, attack detection, investigation, threat hunting, detection engineering, and incident response.

---

# Why Build This SOC Lab?

Cybersecurity can be learned by watching videos or reading documentation, but becoming a Security Operations Center (SOC) Analyst requires practical experience. Most enterprise security environments are not publicly accessible, making it difficult for beginners to understand how security monitoring actually works.

The purpose of this lab is to recreate a realistic SOC environment inside a virtual infrastructure where security events can be generated, collected, monitored, investigated, and documented safely without affecting any production systems.

Instead of installing security tools without understanding their purpose, this project focuses on building every component from the ground up and learning how they communicate with each other.

This lab was designed with the following goals:

- Understand how enterprise SOC environments operate.
- Learn endpoint monitoring using Sysmon.
- Deploy and configure the Wazuh SIEM platform.
- Collect Windows security logs.
- Understand how logs travel from an endpoint to the SIEM.
- Practice real SOC investigations.
- Learn MITRE ATT&CK techniques.
- Perform attack simulations in a safe environment.
- Develop incident investigation skills.
- Create a professional cybersecurity portfolio project.

---

# Why These Technologies Were Selected

Every technology used in this project was selected to represent a specific component commonly found in an enterprise Security Operations Center (SOC).

The objective was not to use as many security tools as possible, but to understand how each technology contributes to the overall security monitoring process.

Together, these technologies create a practical learning environment that closely resembles a real-world SOC while remaining simple enough to build on a personal computer.

---

## VMware Workstation

VMware Workstation was selected to create an isolated virtual environment where multiple operating systems can run simultaneously on a single physical machine.

It provides a safe and flexible platform for building, testing, and managing the Enterprise SOC Lab.

Key reasons for selecting VMware include:

- Support for multiple virtual machines
- Network isolation
- Snapshot and recovery capabilities
- Safe attack simulation
- Easy lab management

---

## Windows 11 Host Machine

Windows 11 was selected as the host operating system because it represents the SOC analyst's workstation.

It is used to manage the virtual lab, access the Wazuh Dashboard, perform investigations, and document findings throughout the project.

In enterprise environments, security analysts typically work from dedicated workstations rather than directly accessing production servers.

---

## Ubuntu Server

Ubuntu Server was selected because it is officially supported by Wazuh, provides excellent stability through Long-Term Support (LTS) releases, and is widely used in enterprise server environments.

It serves as the operating system for the central security monitoring platform used throughout this project.

---

## Windows 10 Endpoint

Windows 10 was selected because it closely represents a typical employee workstation found in many organizations.

It provides a realistic endpoint where normal user activities and simulated attacks can generate security events for monitoring and investigation.

---

## Kali Linux

Kali Linux was selected because it includes a comprehensive collection of penetration testing and security assessment tools.

Throughout this project, it is used to simulate attacker activity in a controlled environment and generate realistic security events for analysis.

---

## Why Sysmon?

The default Windows Event Logs do not always provide the level of detail required for security investigations.

Sysmon extends Windows logging by generating detailed endpoint telemetry, allowing security analysts to better understand system activity and investigate suspicious behavior.

It plays an important role in improving endpoint visibility within the Enterprise SOC Lab.

---

## Why Wazuh?

Wazuh was selected because it is a powerful open-source security platform that provides centralized monitoring, log collection, event analysis, and alerting.

It enables security events from monitored endpoints to be collected, processed, stored, and presented through a single dashboard, making security monitoring and investigations more efficient.
---

# Learning Philosophy

The objective of this project is not simply to install software.

The objective is to understand every layer of the security monitoring pipeline.

For this reason, every major configuration, troubleshooting step, investigation, and lesson learned will be documented throughout this repository.

Whenever an issue occurs, the root cause will be identified before applying a solution. This approach helps develop troubleshooting skills that are essential for SOC analysts and security engineers.

By the end of this project, the reader should not only know how to deploy Wazuh but also understand how enterprise security monitoring operates from endpoint telemetry to incident investigation.

---

# Enterprise SOC vs Our Home Lab

One common misconception among beginners is that a home lab is completely different from a real enterprise environment. While the scale is significantly different, the overall architecture and security workflow remain fundamentally the same.

The objective of this project is not to replicate thousands of production systems, but to understand how enterprise security monitoring works by building a smaller, fully controlled environment.

In a real organization, hundreds or even thousands of employee devices continuously generate security events. These events are collected by endpoint monitoring tools, forwarded to a centralized SIEM platform, analyzed using detection rules, and investigated by SOC analysts.

This project follows the exact same workflow using a small virtual environment.

The only major difference is the number of systems involved.

Instead of monitoring thousands of endpoints, this lab monitors a single Windows endpoint. Instead of multiple SOC analysts working in shifts, a single learner performs both attack simulation and defensive investigation.

Although simplified, every major component of an enterprise SOC is represented inside this lab.

---

## Enterprise Environment vs Home SOC Lab

| Feature | Enterprise Environment | Home SOC Lab |
| ------- | ---------------------- | ------------ |

| **Endpoints** | Thousands of employee endpoints | One Windows 10 virtual machine |

| **Servers** | Multiple production servers | One Ubuntu Server virtual machine |

| **Team/Staff** | Multiple SOC analysts working in shifts | One learner acting as SOC analyst |

| **Threat Actor** | External attackers | Kali Linux virtual machine |

| **SIEM Platform** | Enterprise SIEM platform | Wazuh SIEM |

| **Monitoring Agent** | Endpoint monitoring agent | Wazuh Agent |

| **Telemetry** | Endpoint telemetry | Sysmon |

| **Dashboard** | Security dashboard | Wazuh Dashboard |

| **Network** | Production network | VMware virtual network |

| **Use Cases** | Multiple security investigations | Hands-on investigation exercises |

---

## Similarities

Although this lab is much smaller than an enterprise environment, the security workflow remains the same.

Both environments perform the following tasks:

- Monitor endpoint activities
- Collect Windows security logs
- Forward logs to a centralized server
- Analyze events using a SIEM platform
- Detect suspicious activities
- Investigate alerts
- Perform threat hunting
- Respond to security incidents

The skills learned inside this lab directly apply to real SOC environments because the monitoring process remains fundamentally the same.

---

## Key Difference

The biggest difference between an enterprise SOC and this home lab is scale, not methodology.

Enterprise organizations may monitor:

- Thousands of endpoints
- Hundreds of servers
- Multiple Active Directory domains
- Firewalls
- Cloud infrastructure
- Email gateways
- VPN appliances
- Network security devices

Our home lab focuses on a smaller number of systems while preserving the same investigation workflow.

This approach allows every component to be understood in detail before moving into larger enterprise environments.

---

## Why This Lab Is Valuable

Building a SOC lab manually provides practical experience that cannot be gained by only watching tutorials or reading documentation.

Throughout this project, every component is configured manually, every issue is investigated, and every solution is documented.

This hands-on approach develops the practical troubleshooting, investigation, and analytical skills expected from SOC Analysts and Security Engineers.

By the end of this project, the reader will understand not only how enterprise security monitoring works but also why each component exists and how they interact with each other.

---

# Lab Components

The Enterprise SOC Lab is built using four virtual systems, where each machine performs a dedicated role within the security monitoring pipeline. Although the environment is small, every component represents a real-world enterprise counterpart.

Instead of combining multiple services into a single machine, each role is separated to simulate how modern organizations deploy security infrastructure.

The lab consists of the following components:

1. Windows 11 Host Machine
2. Ubuntu Server
3. Windows 10 Endpoint
4. Kali Linux Attacker Machine

Together, these systems create a complete attack-and-defense environment where attacks can be simulated, security events can be generated, logs can be collected, and investigations can be performed.

---

## Enterprise SOC Lab Architecture

```text
                         Internet
                             │
                             │
                    ┌──────────────────┐
                    │ Kali Linux VM    │
                    │    (Attacker)    │
                    └────────┬─────────┘
                             │
                    VMware Virtual Network
                             │
        ┌────────────────────┼────────────────────┐
        │                    │                    │
        │                    │                    │
┌────────────────┐   ┌─────────────────────┐   ┌────────────────────────┐
│ Windows 10 VM  │   │ Ubuntu Server       │   │ Windows 11 Host         │
│ Employee System│   │ Wazuh Manager       │   │ SOC Analyst Workstation │
│ Sysmon         │──▶│ Filebeat            │◀──│ Web Browser             │
│ Wazuh Agent    │   │ OpenSearch Indexer  │   │ Investigation           │
└────────────────┘   │ Wazuh Dashboard     │   └────────────────────────┘
                     └─────────────────────┘
```

---

# Component 1 — Windows 11 Host Machine

The Windows 11 host machine represents the SOC Analyst's workstation.

This is the physical computer used to operate the entire virtual lab. It hosts VMware Workstation and provides access to all virtual machines running inside the environment.

The SOC analyst uses this system to:

- Access the Wazuh Dashboard
- Perform threat hunting
- Investigate security alerts
- Review Sysmon logs
- Monitor endpoint activity
- Document investigations
- Execute search queries
- Generate reports

This machine is **not monitored** by Wazuh in this project. Its primary purpose is to act as the analyst's workstation.

---

# Component 2 — Ubuntu Server

The Ubuntu Server acts as the central monitoring server of the SOC lab.

It hosts all major Wazuh services responsible for collecting, processing, storing, and visualizing security events.

The server runs:

- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard
- Filebeat

Responsibilities of this server include:

- Receiving logs from Windows endpoints
- Processing security events
- Applying detection rules
- Mapping events to MITRE ATT&CK
- Generating alerts
- Storing logs
- Displaying dashboards

In a real enterprise environment, this role is typically performed by dedicated Linux servers or clustered deployments.

---

# Component 3 — Windows 10 Endpoint

The Windows 10 virtual machine represents an employee workstation inside an organization.

This endpoint is continuously monitored throughout the project.

The following software is installed on this machine:

- Sysmon
- Wazuh Agent

Every activity performed on this endpoint generates security events, including:

- Process creation
- Network activity
- Registry modifications
- File operations
- PowerShell execution
- User activity

The Wazuh Agent forwards these events to the Ubuntu Server for analysis.

Throughout this project, this machine will be used to generate both normal user activity and simulated attack activity.

---

# Component 4 — Kali Linux Attacker

The Kali Linux virtual machine represents the external attacker.

Its purpose is to simulate real-world cyber attacks against the monitored Windows endpoint.

During later phases of this project, Kali Linux will be used to perform:

- Network reconnaissance
- Port scanning
- Service enumeration
- Password attacks
- Exploitation
- Reverse shells
- Post-exploitation
- Persistence techniques

Every attack launched from Kali Linux will generate security events that will be investigated using Wazuh.

---

# Component Relationship

Each component depends on the others to complete the monitoring pipeline.

Kali Linux generates attack traffic.

Windows 10 generates security events.

Sysmon records detailed endpoint activity.

Wazuh Agent collects Windows events.

Ubuntu Server processes and stores the logs.

The SOC Analyst accesses the Wazuh Dashboard from the Windows 11 host machine to investigate these events.

This architecture closely resembles the workflow used in enterprise Security Operations Centers, where security telemetry flows from endpoints to centralized monitoring systems for analysis and investigation.

---

# Security Monitoring Pipeline (Data Flow)

One of the most important concepts in a Security Operations Center (SOC) is understanding how security events travel from an endpoint to the analyst's dashboard.

In this lab, every user activity performed on the Windows 10 endpoint follows a defined monitoring pipeline before becoming available for investigation.

Whenever a user performs an action such as opening an application, executing a command, modifying the registry, or creating a network connection, Windows generates operating system events.

Sysmon extends the default Windows logging by recording detailed telemetry about these activities. These events are written to the Windows Event Log, where they become available for collection.

The Wazuh Agent continuously monitors the Windows Event Log and securely forwards these events to the Ubuntu Server running the Wazuh Manager.

After receiving the events, the Wazuh Manager processes them using decoders and detection rules. Depending on the event, Wazuh may classify it as a normal event or generate a security alert.

The processed data is then forwarded to OpenSearch through Filebeat, where it is indexed and stored for future searching and analysis.

Finally, the Wazuh Dashboard retrieves the indexed data from OpenSearch, allowing the SOC analyst to search, investigate, visualize, and correlate security events.

This end-to-end workflow represents the complete security monitoring pipeline implemented throughout this project.

---

## Security Monitoring Pipeline

```text
+----------------------+
| Windows User Action  |
+----------+-----------+
           │
           ▼
+----------------------+
| Windows Operating    |
| System Activity      |
+----------+-----------+
           │
           ▼
+----------------------+
| Sysmon               |
| (Event Generation)   |
+----------+-----------+
           │
           ▼
+----------------------+
| Windows Event Log    |
| (Event Viewer)       |
+----------+-----------+
           │
           ▼
+----------------------+
| Wazuh Agent          |
| (Log Collection)     |
+----------+-----------+
           │
           ▼
+----------------------+
| Ubuntu Server        |
| Wazuh Manager        |
+----------+-----------+
           │
           ▼
+----------------------+
| Filebeat             |
+----------+-----------+
           │
           ▼
+----------------------+
| OpenSearch Indexer   |
+----------+-----------+
           │
           ▼
+----------------------+
| Wazuh Dashboard      |
+----------+-----------+
           │
           ▼
+----------------------+
| SOC Analyst          |
| Investigation        |
+----------------------+
```

---

## Why Understanding the Data Flow Is Important

Understanding the monitoring pipeline is one of the most important skills for a SOC Analyst.

Whenever an alert is generated, an analyst should know:

- Where the event originated.
- Which component generated the event.
- Which component collected the event.
- Which server processed the event.
- Where the event was stored.
- How the event reached the dashboard.

Knowing this workflow makes troubleshooting significantly easier because each component can be verified individually.

Throughout this project, every investigation will follow this same monitoring pipeline.

---

# Advantages of This Lab

Building an Enterprise SOC Lab in a virtual environment provides hands-on experience that closely resembles real-world security operations. Instead of learning security concepts only through theory, this lab allows every stage of the monitoring pipeline to be observed, configured, and investigated in a controlled environment.

The architecture has been intentionally designed to separate the attacker, monitored endpoint, monitoring server, and analyst workstation. This separation makes it easier to understand how security telemetry moves through the environment and how enterprise SOC teams investigate security events.

The major advantages of this lab include:

- Safe environment for cybersecurity learning.
- Complete control over every virtual machine.
- Practical understanding of enterprise SOC architecture.
- Real-time Windows endpoint monitoring.
- Detailed event collection using Sysmon.
- Centralized log management with Wazuh.
- Hands-on investigation of security events.
- Attack simulation without affecting production systems.
- Opportunity to practice threat hunting.
- Strong foundation for SOC Analyst L1 roles.
- Professional documentation suitable for GitHub and portfolio projects.

One of the biggest advantages of this project is that every configuration, troubleshooting step, and investigation is documented. This transforms the repository from a simple installation guide into a complete learning resource that can be reused in the future.

---

# Current Limitations

Although this lab follows the same workflow as an enterprise Security Operations Center, it is intentionally simplified to make learning easier.

The current implementation includes only a small number of virtual machines and focuses primarily on endpoint monitoring using Wazuh and Sysmon.

Some enterprise components are intentionally excluded at this stage and will be introduced in future phases of the project.

Current limitations include:

- Single monitored Windows endpoint.
- Single Ubuntu monitoring server.
- Single attacker machine.
- No Active Directory environment.
- No Windows Server Domain Controller.
- No firewall log integration.
- No IDS/IPS integration.
- No cloud infrastructure monitoring.
- No email security gateway.
- No EDR/XDR platform.
- No multi-site network topology.
- No high-availability Wazuh cluster.

These limitations do not reduce the educational value of the project. Instead, they allow the core concepts of security monitoring and investigation to be understood before introducing more complex enterprise technologies.

Future versions of this lab will gradually expand the architecture by adding new enterprise components and attack scenarios.

---

# Summary

This document introduced the complete architecture of the Enterprise SOC Lab and explained the purpose of every major component within the environment.

The lab has been designed to simulate the workflow of a modern Security Operations Center while remaining small enough to be built and understood on a personal computer.

Throughout this chapter, we explored:

- The overall objective of the project.
- Why this SOC lab was created.
- The technologies selected for the environment.
- The differences between an enterprise SOC and a home lab.
- The role of every virtual machine.
- The complete security monitoring pipeline.
- How security events travel from the endpoint to the analyst's dashboard.

Understanding this architecture is essential because every future chapter depends on the concepts introduced here.

As the project progresses, this architecture will serve as the foundation for endpoint monitoring, attack simulation, threat hunting, detection engineering, and incident response.

---

# Key Takeaways

After completing this chapter, the reader should understand:

- The overall purpose of the Enterprise SOC Lab.
- The role of each virtual machine.
- Why Ubuntu Server hosts the Wazuh platform.
- Why Windows 10 is used as the monitored endpoint.
- Why Sysmon is required for detailed Windows telemetry.
- Why Kali Linux represents the attacker.
- How the Wazuh Agent forwards logs to the Wazuh Manager.
- How Filebeat indexes processed events.
- How OpenSearch stores security events.
- How the Wazuh Dashboard displays searchable security data.
- How a SOC Analyst investigates endpoint activity using centralized logging.
- Why understanding the complete monitoring pipeline is critical for effective troubleshooting and incident investigation.

These concepts form the foundation for every practical exercise and investigation performed throughout this repository.

---

# Next Chapter

The next chapter introduces the prerequisites required to build this Enterprise SOC Lab.

It covers:

- Hardware requirements
- Software requirements
- System specifications
- Virtualization requirements
- ISO images
- Network configuration
- Required downloads
- Tools used throughout the project

Before installing any operating system or security tool, it is important to understand the resources required to build a stable and reliable virtual lab.

This preparation helps avoid installation issues and ensures that every component functions correctly throughout the remainder of the project.

**Next Document: `02-Prerequisites.md`**

# Document Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | July 2026 | Rajeev Kumar | Initial release of the Enterprise SOC Lab Architecture documentation. |