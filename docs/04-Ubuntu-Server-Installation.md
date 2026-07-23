# Ubuntu Server Installation

---

# Chapter Information

| Property                   | Details                              |
| -------------------------- | ------------------------------------ |
| **Chapter Number**         | 04                                   |
| **Chapter Title**          | Ubuntu Server Installation           |
| **Difficulty**             | Beginner                             |
| **Estimated Reading Time** | 15–20 Minutes                        |
| **Estimated Lab Time**     | 25–35 Minutes                        |
| **Prerequisite**           | Chapter 03 – Virtual Lab Setup       |
| **Category**               | Installation Guide                   |
| **Related Troubleshooting**| Troubleshooting.md                   |
| **Next Chapter**           | Chapter 05 – Windows 10 Installation |

---

# Document Information

| Property          | Details                    |
| ----------------- | -------------------------- |
| **Project**       | Enterprise SOC Lab         |
| **Document Name** | Ubuntu Server Installation |
| **Version**       | 1.0                        |
| **Author**        | Rajeev Rock               |
| **Status**        | Published                  |
| **Last Updated**  | July 2026                  |

---

# Learning Objectives

After completing this chapter, you will be able to:

* Download the Ubuntu Server LTS ISO image from the official source.
* Verify that the installation media is ready for deployment.
* Prepare the required files for virtual machine creation.
* Install Ubuntu Server using the recommended configuration.
* Perform basic post-installation verification.
* Update the operating system before deploying additional software.

---

# Objective

Install and verify Ubuntu Server for the Enterprise SOC Lab.

This server will be used in later chapters to deploy the Wazuh platform. This chapter covers only the operating system installation, initial configuration, verification, and system update.

---

# Download Ubuntu Server

Download the latest Ubuntu Server LTS ISO from the official Ubuntu website and save it in your project directory for use during virtual machine creation.

## Official Download Source

| Resource               | Link                               |
| ---------------------- | ---------------------------------- |
| Ubuntu Server Download | https://ubuntu.com/download/server |

## Download Requirements

| Component    | Recommended Value |
| ------------ | ----------------- |
| Edition      | Ubuntu Server LTS |
| Architecture | 64-bit (AMD64)    |
| File Format  | ISO Image         |

Store the downloaded ISO in a dedicated project directory so it can be easily accessed during virtual machine creation.

**Example Directory Structure**

```text
Enterprise-SOC-Lab/
│
├── Images/
│   ├── ubuntu-server.iso
│   ├── windows10-vmware.zip
│   └── kali-linux-vmware.7z
│
├── VMware/
├── docs/
└── screenshots/
```

## Verification

Before continuing, verify the following:

* The ISO download completed successfully.
* The file can be opened from the selected directory.
* The file size matches the value displayed on the Ubuntu download page.
* The ISO is ready to be attached to the virtual machine.

> **📸 Screenshot Required**
>
> **Title:** Ubuntu Server ISO Download
>
> **Save As:** `screenshots/ubuntu/01-ubuntu-server-download.png`

# Create the Ubuntu Virtual Machine

Create a new VMware virtual machine using the Ubuntu Server ISO.

Using a separate virtual machine provides an isolated environment for the Enterprise SOC Lab and simplifies future management and maintenance.

## VMware Version

This guide uses VMware Workstation Pro. Any recent version that supports Ubuntu Server 24.04 LTS (64-bit) can be used.

## Virtual Machine Configuration

Create a new virtual machine using the following configuration.

| Setting                | Recommended Value            |
| ---------------------- | ---------------------------- |
| Configuration Type     | Typical (Recommended)        |
| Installer Source       | Ubuntu Server ISO            |
| Guest Operating System | Linux                        |
| Version                | Ubuntu 64-bit                |
| Virtual Machine Name   | `wazuh-server`               |
| Location               | Default or Project Directory |

After completing the wizard, proceed to configure the virtual hardware before powering on the virtual machine.

> **📸 Screenshot Required**
>
> **Title:** Create Ubuntu Virtual Machine
>
> **Save As:** `screenshots/ubuntu/02-create-virtual-machine.png`

---

# Configure Virtual Machine Hardware

Configure the virtual hardware using the recommended settings below before powering on the virtual machine.

## Hardware Configuration

| Component       | Recommended Configuration    |
| --------------- | ---------------------------- |
| Processors      | 2 vCPUs                      |
| Memory          | 4 GB RAM                     |
| Hard Disk       | 60 GB (Single Virtual Disk) |
| Network Adapter | NAT                          |
| CD/DVD Drive    | Ubuntu Server ISO            |
| USB Controller  | Default                      |
| Sound Card      | Default (Optional)           |

Review the hardware settings before powering on the virtual machine to ensure the installation media is attached correctly.

> **📸 Screenshot Required**
>
> **Title:** Virtual Machine Hardware Settings
>
> **Save As:** `screenshots/ubuntu/03-vm-hardware-settings.png`

---

# Install Ubuntu Server

Boot the virtual machine from the Ubuntu Server ISO and complete the installation using the recommended configuration below.


## Installation Configuration

| Configuration         | Recommended Selection               |
| --------------------- | ----------------------------------- |
| Installation Language | English                             |
| Keyboard Layout       | English (US)                        |
| Installation Type     | Ubuntu Server                       |
| Network Configuration | Automatic (DHCP)                    |
| Proxy Configuration   | Leave Blank (If Not Required)       |
| Mirror Address        | Default Ubuntu Mirror               |
| Storage Layout        | Use Entire Disk                     |
| Storage Configuration | Guided Storage                      |
| Profile Setup         | Configure Hostname and User Account |
| OpenSSH Server        | Install OpenSSH Server              |
| Featured Server Snaps | Skip                                |

Review the installation summary, begin the installation, and restart the virtual machine when prompted.

# Configure Ubuntu Server

During the installation process, Ubuntu Server prompts for basic system information. Configure the server using the recommended values below.

## Hostname

Configure the server hostname as shown below.

| Setting | Recommended Value |
| --------| ----------------- |
| Hostname | `wazuh-server` |

Choose a descriptive hostname that reflects the server's role. This simplifies administration when managing multiple systems.

---

## Username

Create a standard administrative user for daily management tasks.

| Setting   | Recommended Value       |
| --------- | ----------------------- |
| Full Name | Your Name               |
| Username  | Your Preferred Username |

Use a simple, consistent username that will be used throughout the lab.

---

## Password

Use a strong password for the administrative user.

| Setting          | Recommendation  |
| ---------------- | --------------- |
| Password         | Strong Password |
| Confirm Password | Same as Above   |

A strong password should include a combination of:

* Uppercase letters
* Lowercase letters
* Numbers
* Special characters

---

## OpenSSH

Enable the OpenSSH Server during installation.

| Setting                | Recommended Selection |
| ---------------------- | --------------------- |
| Install OpenSSH Server | Yes                   |

Installing OpenSSH allows secure remote administration in later chapters.

---

# Verify the Installation

After restarting the virtual machine, sign in and run the following commands to verify the installation.

## Verification Commands

```bash
hostname
```

```bash
hostnamectl
```

```bash
lsb_release -a
```

```bash
ip a
```

```bash
ping -c 4 8.8.8.8
```

## Expected Results

| Verification          | Expected Result                                    |
| --------------------- | -------------------------------------------------- |
| Login                 | Successful login using the configured user account |
| Hostname              | Displays the configured hostname                   |
| Operating System      | Ubuntu Server LTS is displayed                     |
| Network               | A valid IP address is assigned                     |
| Internet Connectivity | Ping completes successfully with replies           |

> **📸 Screenshot Required**
>
> **Title:** First Login
>
> **Save As:** `screenshots/ubuntu/04-first-login.png`

---

# Update Ubuntu Server

Update the operating system before continuing with the lab.

```bash
sudo apt update
```

```bash
sudo apt upgrade -y
```

```bash
sudo reboot
```

After rebooting, verify that the system starts normally and the user account can log in successfully.

> **📸 Screenshot Required**
>
> **Title:** Ubuntu System Update
>
> **Save As:** `screenshots/ubuntu/5-system-update.png`

# Installation Complete

Ubuntu Server is now installed, verified, and updated. The system is ready for Wazuh deployment in the next chapter.

---

# Installation Checklist

Verify that each task has been completed successfully before moving to the next chapter.

| Status | Task                                    |
| ------ | --------------------------------------- |
| ☐      | Ubuntu Server installed successfully    |
| ☐      | Virtual machine boots without errors    |
| ☐      | User account created and login verified |
| ☐      | Hostname configured correctly           |
| ☐      | OpenSSH Server installed                |
| ☐      | Network connectivity verified           |
| ☐      | Internet access confirmed               |
| ☐      | System packages updated                 |
| ☐      | Server reboot completed successfully    |

---

# Common Installation Issues

| Issue                         | Possible Cause                                      | Resolution                                                              |
| ----------------------------- | --------------------------------------------------- | ----------------------------------------------------------------------- |
| Virtual machine does not boot | ISO not attached or incorrect boot order            | Verify the ISO attachment and boot order in VMware settings.            |
| Unable to log in              | Incorrect username or password                      | Verify the credentials created during installation and check Caps Lock. |
| No IP address assigned        | Network adapter configuration                       | Confirm the network adapter is enabled and configured for NAT.          |
| No internet connectivity      | VMware network service or host network issue        | Verify the NAT configuration and test the host's internet connection.   |
| OpenSSH not available         | OpenSSH Server was not selected during installation | Install OpenSSH after installation using the package manager.           |
| System update fails           | Network connectivity or package repository issue    | Verify internet connectivity and run the update commands again.         |

---

# Summary

In this chapter, you installed Ubuntu Server, configured the virtual machine, verified the installation, and updated the operating system. The server is now ready for the Wazuh installation.

---

# Key Takeaways

- Download and prepare the Ubuntu Server LTS ISO.
- Create a VMware virtual machine.
- Install Ubuntu Server with the recommended configuration.
- Verify the installation.
- Update the operating system before deploying Wazuh.
---

# Next Chapter

The next chapter covers the installation and initial configuration of Windows 10, which will be used as the monitored endpoint in the Enterprise SOC Lab.

**Topics Covered**

* Create the Windows 10 virtual machine
* Install Windows 10
* Perform the initial operating system configuration
* Verify network connectivity
* Prepare the endpoint for Wazuh Agent installation

**Next Document**

`05 - Windows10-Installation.md`

---

# Document Revision History

| Version | Date      | Author       | Description                                              |
| ------- | --------- | ------------ | -------------------------------------------------------- |
| 1.0     | July 2026 | Rajeev Rock | Initial release of the Ubuntu Server Installation guide. |
