# Wazuh Installation

---

# Chapter Information

| Property                   | Details                               |
| -------------------------- | ------------------------------------- |
| **Chapter Number**         | 07                                    |
| **Chapter Title**          | Wazuh Installation                    |
| **Difficulty**             | Beginner → Intermediate               |
| **Estimated Reading Time** | 20–25 Minutes                         |
| **Estimated Lab Time**     | 30–45 Minutes                         |
| **Prerequisite**           | Chapter 06 – Kali Linux Installation  |
| **Category**               | Installation Guide                    |
| **Related Troubleshooting**| Troubleshooting.md                    |
| **Next Chapter**           | Chapter 08 – Windows Agent Deployment |

---

# Document Information

| Property          | Details            |
| ----------------- | ------------------ |
| **Project**       | Enterprise SOC Lab |
| **Document Name** | Wazuh Installation |
| **Version**       | 1.0                |
| **Author**        | Rajeev Kumar       |
| **Status**        | Published          |
| **Last Updated**  | July 2026          |

---

# Learning Objectives

After completing this chapter, you will be able to:

* Understand the role of Wazuh in the Enterprise SOC Lab.
* Identify the core components of the Wazuh platform.
* Install Wazuh using the official installation method.
* Verify that the required Wazuh services are running successfully.
* Access the Wazuh Dashboard through a web browser.
* Prepare the platform for endpoint onboarding.

---

# Objective

This chapter explains how to install and verify a single-node Wazuh deployment on the Ubuntu Server using the official Wazuh Installation Assistant.

The chapter covers only the installation, verification, and initial dashboard access. Windows Agent deployment and additional configuration are covered in later chapters.
---

# What is Wazuh?

Wazuh is an open-source security platform for endpoint monitoring, log collection, threat detection, and security event analysis.

In this lab, it serves as the central platform for collecting and analyzing security events from monitored endpoints.

---

# Wazuh Components

The Enterprise SOC Lab uses a **single-node Wazuh deployment** consisting of the following components.

| Component              | Purpose                                                                     |
| ---------------------- | --------------------------------------------------------------------------- |
| **Wazuh Manager**      | Receives, processes, and analyzes security events from monitored endpoints. |
| **OpenSearch Indexer** | Stores and indexes collected security data for fast searching.              |
| **Wazuh Dashboard**    | Provides a web interface for monitoring, investigation, and visualization.  |
| **Filebeat**           | Forwards Wazuh alerts and archives to the OpenSearch Indexer.               |

All components are installed on the same Ubuntu Server, providing a simple deployment while maintaining an enterprise-style architecture.

---

# Installation Prerequisites

Before starting the installation, verify that the following requirements have been completed.

| Requirement                       | Status |
| --------------------------------- | :----: |
| Ubuntu Server installed           |    ✓   |
| Ubuntu Server updated             |    ✓   |
| Internet connectivity available   |    ✓   |
| Root or sudo privileges available |    ✓   |
| VMware virtual machine running    |    ✓   |

Completing these prerequisites helps ensure a smooth installation and reduces the likelihood of deployment issues.

---

# Installation Method

This project uses the **official Wazuh Installation Assistant** to deploy a **single-node** Wazuh environment.

The installation assistant automatically installs and configures:

* Wazuh Manager
* OpenSearch Indexer
* Wazuh Dashboard
* Filebeat

Using the official installation method ensures component compatibility, simplifies deployment, and aligns with the recommended Wazuh installation procedure.

---

# Install Wazuh

Run the following commands on the Ubuntu Server terminal.

## Step 1 — Download the Installation Assistant

```bash
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh
```

## Step 2 — Make the Script Executable

```bash
chmod +x wazuh-install.sh
```

## Step 3 — Install Wazuh

```bash
sudo ./wazuh-install.sh -a
```

The installation assistant automatically installs and configures:

* Wazuh Manager
* OpenSearch Indexer
* Wazuh Dashboard
* Filebeat

Depending on the allocated virtual machine resources and internet connection, the installation typically takes **20–40 minutes**.

---

# Installation Output

After the installation completes successfully, the Wazuh Installation Assistant displays important deployment information.

The output includes:

* Wazuh Dashboard URL
* Administrator username
* Generated administrator password
* Installation status

Example:

```text
Wazuh Dashboard:
https://<Ubuntu-Server-IP>

Username: admin
Password: <Generated Password>
```

Record the generated administrator password before closing the terminal. It is required to sign in to the Wazuh Dashboard.

---

# Dashboard Credentials

Use the credentials generated during the installation to access the Wazuh Dashboard.

| Credential    | Value                         |
| ------------- | ----------------------------- |
| Dashboard URL | `https://<Ubuntu-Server-IP>`  |
| Username      | `admin`                       |
| Password      | Generated during installation |

> **Important**
>
> Store the generated administrator password in a secure location. It will be required throughout the remaining chapters of the Enterprise SOC Lab.

# Access Wazuh Dashboard

After the installation completes successfully, open a web browser on your host machine and navigate to the Wazuh Dashboard.

If the browser displays a security warning because of the self-signed certificate, proceed to the website.

Sign in using the administrator credentials generated during the installation.


> **📸 Screenshot Required**
>
> **Title:** Wazuh Dashboard Login
>
> **Save As:** `screenshots/wazuh/01-dashboard-login.png`

---

# Verify Dashboard

The dashboard should load successfully and display:

- Dashboard Home
- Navigation menu
- Wazuh Overview
- No critical startup errors

> **📸 Screenshot Required**
>
> **Title:** Wazuh Dashboard Overview
>
> **Save As:** `screenshots/wazuh/02-dashboard-overview.png`

---

# Verify Installed Services

Verify that all required Wazuh services are running on the Ubuntu Server.

| Service         | Verification Command                    | Expected Status    |
| --------------- | --------------------------------------- | ------------------ |
| Wazuh Manager   | `sudo systemctl status wazuh-manager`   | `active (running)` |
| Wazuh Indexer   | `sudo systemctl status wazuh-indexer`   | `active (running)` |
| Wazuh Dashboard | `sudo systemctl status wazuh-dashboard` | `active (running)` |
| Filebeat        | `sudo systemctl status filebeat`        | `active (running)` |

If any service is not running, review its status before continuing to the next chapter.

> **📸 Screenshot Required**
>
> **Title:** Wazuh Services Status
>
> **Save As:** `screenshots/wazuh/03-services-status.png`

---

# Verify Network Connectivity

Verify that the Ubuntu Server is connected to the network and can communicate successfully.

## Verification Commands

```bash
hostname
```

```bash
ip a
```

```bash
ping -c 4 8.8.8.8
```

## Expected Results

| Verification          | Expected Result                          |
| --------------------- | ---------------------------------------- |
| Hostname              | Displays the configured server hostname  |
| Network               | A valid IP address is assigned           |
| Internet Connectivity | Ping completes successfully with replies |

> **📸 Screenshot Required**
>
> **Title:** Network Verification
>
> **Save As:** `screenshots/wazuh/04-network-verification.png`

---

# Installation Verification

Before proceeding to the next chapter, verify that the Wazuh deployment is operating correctly.

| Component            | Expected Status |
| -------------------- | --------------- |
| Wazuh Manager        | Running         |
| Wazuh Indexer        | Running         |
| Wazuh Dashboard      | Running         |
| Filebeat             | Running         |
| Dashboard Login      | Successful      |
| Network Connectivity | Available       |

If all components are operating as expected, the Wazuh platform has been installed successfully and is ready for Windows Agent deployment.

# Installation Checklist

Before proceeding to the next chapter, verify that the Wazuh platform has been installed successfully.

| Status | Task                                               |
| ------ | -------------------------------------------------- |
| ☐      | Wazuh Installation Assistant executed successfully |
| ☐      | Wazuh Manager service is running                   |
| ☐      | Wazuh Indexer service is running                   |
| ☐      | Wazuh Dashboard service is running                 |
| ☐      | Filebeat service is running                        |
| ☐      | Wazuh Dashboard is accessible                      |
| ☐      | Administrator login verified                       |
| ☐      | Ubuntu Server has network connectivity             |
| ☐      | Wazuh platform is ready for agent deployment       |

---

# Common Installation Issues

| Issue                                   | Possible Cause                                      | Resolution                                                                                |
| --------------------------------------- | --------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| Installation script fails               | Internet connectivity issue or interrupted download | Verify the network connection and rerun the installation assistant.                       |
| Dashboard not accessible                | Wazuh Dashboard service is not running              | Verify the service status and restart the service if necessary.                           |
| Login failed                            | Incorrect administrator credentials                 | Use the credentials generated during installation.                                        |
| One or more services are inactive       | Installation did not complete successfully          | Review the service status and installation logs before retrying.                          |
| Browser security warning                | Self-signed SSL certificate                         | Accept the certificate warning and continue to the dashboard.                             |
| Unable to access the dashboard remotely | Network configuration or firewall restriction       | Verify the Ubuntu Server IP address, VMware network settings, and firewall configuration. |

---

# Summary

In this chapter, you installed a single-node Wazuh platform using the official Installation Assistant, verified the required services, and confirmed dashboard accessibility. The platform is now ready for Windows Agent deployment.

---

# Key Takeaways

After completing this chapter, you can:

* Deploy Wazuh using the official Installation Assistant.
* Access the Wazuh Dashboard.
* Verify the status of the required Wazuh services.
* Confirm dashboard accessibility and network connectivity.
* Prepare the platform for Windows Agent deployment.

---

# Next Chapter

The next chapter focuses on deploying the Wazuh Agent on the Windows 10 endpoint.

**Topics Covered**

* Downloading the Wazuh Windows Agent
* Installing the Windows Agent
* Registering the endpoint with the Wazuh Manager
* Verifying agent connectivity
* Confirming endpoint visibility in the Wazuh Dashboard

**Next Document**

`08 - Windows-Agent-Deployment.md`

---

# Document Revision History

| Version | Date      | Author       | Description                                      |
| ------- | --------- | ------------ | ------------------------------------------------ |
| 1.0     | July 2026 | Rajeev Kumar | Initial release of the Wazuh Installation guide. |
