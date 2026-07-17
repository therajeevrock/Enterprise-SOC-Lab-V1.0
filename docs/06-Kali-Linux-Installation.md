# Kali Linux Installation

---

# Chapter Information

| Property                   | Details                              |
| -------------------------- | ------------------------------------ |
| **Chapter Number**         | 06                                   |
| **Chapter Title**          | Kali Linux Installation              |
| **Difficulty**             | Beginner                             |
| **Estimated Reading Time** | 15–20 Minutes                        |
| **Estimated Lab Time**     | 30–40 Minutes                        |
| **Prerequisite**           | Chapter 05 – Windows 10 Installation |
| **Category**               | Installation Guide                   |
| **Next Chapter**           | Chapter 07 – Wazuh Installation      |

---

# Document Information

| Property          | Details                 |
| ----------------- | ----------------------- |
| **Project**       | Enterprise SOC Lab      |
| **Document Name** | Kali Linux Installation |
| **Version**       | 1.0                     |
| **Author**        | Rajeev Kumar            |
| **Status**        | Published               |
| **Last Updated**  | July 2026               |

---

# Learning Objectives

After completing this chapter, you will be able to:

- Download the official Kali Linux VMware image.
- Extract the downloaded archive.
- Import the Kali Linux virtual machine into VMware Workstation.
- Verify the virtual machine hardware configuration.
- Boot Kali Linux successfully.
- Verify network connectivity.
- Update Kali Linux before beginning attack simulations.

---

# Objective

This chapter explains how to deploy Kali Linux using the official pre-built VMware image provided by the Kali Linux project.

Unlike a traditional ISO installation, the VMware image is already configured and optimized for virtualization. This significantly reduces deployment time and allows the lab environment to be prepared quickly.

Kali Linux will serve as the attacker machine throughout the Enterprise SOC Lab. It will be used to perform controlled attack simulations against the Windows 10 endpoint, enabling the Wazuh platform to collect, analyze, and detect security events.

This chapter focuses only on deploying, configuring, verifying, and updating Kali Linux. The usage of penetration testing tools and attack simulations will be covered in later chapters.

---

# Download Kali Linux

Download the official pre-built VMware image from the Kali Linux website.

The VMware image already contains a fully installed Kali Linux operating system and is optimized for VMware Workstation. Using this image eliminates the need to perform a manual operating system installation.

## Official Download Source

| Resource | Link |
|----------|-------------|
| Kali Linux VMware Image | https://www.kali.org/get-kali/#kali-virtual-machines |

## Download Requirements

| Component | Recommended Value |
|----------|-------------------|
| Distribution | Kali Linux |
| Release | Latest Stable Release |
| Architecture | 64-bit (x86_64) |
| Image Type | VMware Pre-built Image (.7z / .zip) |

Store the downloaded archive inside the project directory.

**Example Directory Structure**

```text
Enterprise-SOC-Lab/
│
├── Images/
│   ├── ubuntu-server.iso
│   ├── windows10-VMware.zip
│   └── kali-linux-vmware.7z
│
├── VMware/
├── docs/
└── screenshots/
```

## Verification

Before continuing, verify the following:

- The VMware image has been downloaded successfully.
- The archive file is not corrupted.
- The image is stored inside the project directory.
- VMware Workstation is installed.

> **📸 Screenshot Required**
>
> **Title:** Kali Linux VMware Download
>
> **Save As:** `screenshots/kali/01-kali-vmware-download.png`

# Import the Kali Linux Virtual Machine

Extract the downloaded Kali Linux VMware archive to your project directory.

After extraction, locate the virtual machine files and open the `.vmx` file using VMware Workstation.

## VMware Version

This guide was prepared using VMware Workstation.

Any recent VMware Workstation version that supports the Kali Linux VMware image can be used.

## Virtual Machine Configuration

| Setting              | Recommended Value             |
| -------------------- | ----------------------------- |
| Deployment Method    | Open Existing Virtual Machine |
| Virtual Machine File | `.vmx`                        |
| Virtual Machine Name | `kali-linux`                  |
| Location             | Project Directory             |

After importing the virtual machine, review the hardware configuration before powering it on.

> **📸 Screenshot Required**
>
> **Title:** Import Kali Linux Virtual Machine
>
> **Save As:** `screenshots/kali/02-import-vm.png`

---

# Configure Virtual Machine Hardware

Verify that the imported virtual machine uses the recommended hardware configuration.

## Hardware Configuration

| Component       | Recommended Configuration |
| --------------- | ------------------------- |
| Processors      | 3 vCPUs                   |
| Memory          | 6 GB RAM                  |
| Hard Disk       | Default Virtual Disk      |
| Network Adapter | NAT                       |
| Display         | Default                   |
| USB Controller  | Default                   |

If necessary, adjust the hardware resources to match your host system.

> **📸 Screenshot Required**
>
> **Title:** Kali Linux Hardware Settings
>
> **Save As:** `screenshots/kali/03-vm-hardware-settings.png`


## Optional VMware Settings

The following virtualization options were left **unchecked** during the deployment of the Kali Linux virtual machine because they are not required for the Enterprise SOC Lab.

| VMware Option                                | Status    | Reason                                                                                    |
| -------------------------------------------- | --------- | ----------------------------------------------------------------------------------------- |
| Virtualize Intel VT-x/EPT or AMD-V/RVI       | Unchecked | Nested virtualization is not required for the Kali Linux virtual machine.                 |
| Virtualize CPU Performance Counters          | Unchecked | Performance counter virtualization is unnecessary for this lab environment.               |
| Virtualize IOMMU (IO Memory Management Unit) | Unchecked | PCI device passthrough and advanced virtualization features are not used in this project. |

These settings can remain at their default values without affecting the functionality of the Enterprise SOC Lab.

> **📸 Screenshot Required**
>
> **Title:** VMware Virtualization Engine Settings
>
> **Save As:** `screenshots/kali/04-virtualization-engine-settings.png`

---

# First Boot

Power on the virtual machine.

When VMware prompts whether the virtual machine was **Moved** or **Copied**, select **I Copied It**.

Log in using the default Kali Linux credentials.

| Setting  | Value  |
| -------- | ------ |
| Username | `kali` |
| Password | `kali` |

> **Note**
>
> If your downloaded Kali image uses different default credentials, refer to the release notes provided with that image.

> **📸 Screenshot Required**
>
> **Title:** Kali Linux Login
>
> **Save As:** `screenshots/kali/05-first-login.png`

---

# Verify the Installation

Open a terminal and run the following commands.

## Verification Commands

```bash
hostname
```

```bash
whoami
```

```bash
ip a
```

```bash
cat /etc/os-release
```

```bash
ping -c 4 google.com
```

## Expected Results

| Verification          | Expected Result                |
| --------------------- | ------------------------------ |
| Login                 | Successful                     |
| Username              | `kali` (or configured user)    |
| Hostname              | Displays the system hostname   |
| Operating System      | Kali Linux is displayed        |
| Network               | A valid IP address is assigned |
| Internet Connectivity | Ping completes successfully    |

> **📸 Screenshot Required**
>
> **Title:** Kali Linux Verification
>
> **Save As:** `screenshots/kali/06-installation-verification.png`

---

# Update Kali Linux

Update all installed packages before using the system.

```bash
sudo apt update
```

```bash
sudo apt full-upgrade -y
```

```bash
sudo reboot
```

After the system restarts, log in again and verify that Kali Linux boots successfully.

> **📸 Screenshot Required**
>
> **Title:** Kali Linux System Update
>
> **Save As:** `screenshots/kali/07-system-update.png`

---

# Installation Complete

Kali Linux has now been successfully deployed, verified, and updated.

The virtual machine is ready for use as the attack simulation system in the Enterprise SOC Lab.

---

# Installation Checklist

| Status | Task                                 |
| ------ | ------------------------------------ |
| ☐      | VMware image downloaded              |
| ☐      | Archive extracted successfully       |
| ☐      | Virtual machine imported into VMware |
| ☐      | Login verified                       |
| ☐      | Network connectivity verified        |
| ☐      | System updated                       |
| ☐      | Kali Linux ready for use             |

---

# Common Installation Issues

| Issue                   | Possible Cause                    | Resolution                                               |
| ----------------------- | --------------------------------- | -------------------------------------------------------- |
| VM does not start       | Incomplete extraction             | Extract the archive again.                               |
| Login failed            | Incorrect credentials             | Verify the default credentials for the downloaded image. |
| No network connectivity | Incorrect VMware network settings | Verify the network adapter is configured for NAT.        |
| Update failed           | Internet connectivity issue       | Verify the network connection and retry the update.      |

---

# Summary

In this chapter, you downloaded the official Kali Linux VMware image, imported it into VMware Workstation, verified the deployment, and updated the operating system.

The attack machine is now ready for use in later chapters.

---

# Key Takeaways

* Download the official Kali Linux VMware image.
* Import the virtual machine into VMware Workstation.
* Verify the deployment.
* Confirm network connectivity.
* Update the operating system before performing security assessments.

---

# Next Chapter

The next chapter focuses on installing the Wazuh platform on the Ubuntu Server created earlier in the Enterprise SOC Lab.

**Next Document**

`07 - Wazuh Installation.md`

---

# Document Revision History

| Version | Date      | Author       | Description                                           |
| ------- | --------- | ------------ | ----------------------------------------------------- |
| 1.0     | July 2026 | Rajeev Kumar | Initial release of the Kali Linux Installation guide. |
