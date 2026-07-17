# Windows 10 Installation

---

# Chapter Information

| Property                   | Details                                 |
| -------------------------- | --------------------------------------- |
| **Chapter Number**         | 05                                      |
| **Chapter Title**          | Windows 10 Installation                 |
| **Difficulty**             | Beginner                                |
| **Estimated Reading Time** | 15–20 Minutes                           |
| **Estimated Lab Time**     | 30–40 Minutes                           |
| **Prerequisite**           | Chapter 04 – Ubuntu Server Installation |
| **Category**               | Installation Guide                      |
| **Related Troubleshooting**| Troubleshooting.md                      |
| **Next Chapter**           | Chapter 06 – Kali-Linux-Installation    |

---

# Document Information

| Property          | Details                 |
| ----------------- | ----------------------- |
| **Project**       | Enterprise SOC Lab      |
| **Document Name** | Windows 10 Installation |
| **Version**       | 1.0                     |
| **Author**        | Rajeev Kumar            |
| **Status**        | Published               |
| **Last Updated**  | July 2026               |

---

# Learning Objectives

After completing this chapter, you will be able to:

* Download the Windows 10 ISO from the official Microsoft website.
* Verify that the installation media is ready for deployment.
* Create a Windows 10 virtual machine in VMware Workstation.
* Install Windows 10 using the recommended configuration.
* Perform the initial operating system configuration.
* Verify that Windows 10 is functioning correctly.
* Update Windows 10 before installing additional software.

---

# Objective

This chapter provides the procedure for installing Windows 10 for the Enterprise SOC Lab.

Windows 10 will function as the monitored endpoint throughout the lab environment. It will later be onboarded to the Wazuh platform for endpoint monitoring, log collection, and security event analysis.

This chapter focuses only on downloading, installing, configuring, and verifying Windows 10. The installation of the Wazuh Agent, Sysmon, and other security tools will be covered in separate chapters.

---

> **Important**
>
> During the development of the Enterprise SOC Lab, a pre-configured Windows 10 virtual machine was used to accelerate the initial lab setup.
>
> However, this guide documents the installation process using the official Microsoft Windows 10 ISO so that readers can build the lab from scratch using a clean and reproducible installation method.

---

# Download Windows 10

Download the Windows 10 ISO image from the official Microsoft website. Using the official source ensures that the installation media is genuine, up to date, and free from unauthorized modifications.

## Official Download Source

| Resource            | Link                                                     |
| ------------------- | -------------------------------------------------------- |
| Windows 10 Download | https://www.microsoft.com/software-download/windows10ISO |

## Download Requirements

| Component          | Recommended Value           |
| ------------------ | --------------------------- |
| Operating System   | Windows 10 Pro              |
| Architecture       | 64-bit (x64)                |
| Installation Media | ISO Image                   |
| License            | Evaluation or Licensed Copy |

Store the downloaded ISO in your project directory so it is readily available during virtual machine creation.

**Example Directory Structure**

```text
Enterprise-SOC-Lab/
│
├── Images/
│   ├── ubuntu-server.iso
│   ├── windows10.iso
│   └── kali-linux-vmware.7z
│
├── VMware/
├── docs/
└── screenshots/
```

## Verification

Before proceeding to VMware Workstation, verify the following:

* The Windows 10 ISO has been downloaded successfully.
* The ISO file is accessible from the project directory.
* The downloaded file size matches the value shown on the Microsoft download page.
* The ISO is ready to be attached as the installation media.

> **📸 Screenshot Required**
>
> **Title:** Windows 10 ISO Download
>
> **Save As:** `screenshots/windows/01-windows10-download.png`

# Create the Windows 10 Virtual Machine

Create a dedicated virtual machine in VMware Workstation for the Windows 10 installation.

This virtual machine will serve as the monitored endpoint in the Enterprise SOC Lab. It will later be onboarded to the Wazuh platform for endpoint monitoring and security event collection.

## VMware Version

This guide was prepared using VMware Workstation.

Any recent VMware Workstation version that supports **Windows 10 (64-bit)** is suitable for this deployment.

## Virtual Machine Configuration

Create a new virtual machine using the following configuration.

| Setting                | Recommended Value            |
| ---------------------- | ---------------------------- |
| Configuration Type     | Typical (Recommended)        |
| Installer Source       | Windows 10 ISO               |
| Guest Operating System | Microsoft Windows            |
| Version                | Windows 10 x64               |
| Virtual Machine Name   | `windows10-client`           |
| Location               | Default or Project Directory |

After completing the wizard, proceed to configure the virtual hardware before powering on the virtual machine.

> **📸 Screenshot Required**
>
> **Title:** Create Windows 10 Virtual Machine
>
> **Save As:** `screenshots/windows/02-create-virtual-machine.png`

---

# Configure Virtual Machine Hardware

Before installing Windows 10, configure the virtual machine with the recommended hardware resources.

## Hardware Configuration

| Component       | Recommended Configuration    |
| --------------- | ---------------------------- |
| Processors      | 2 vCPUs                      |
| Memory          | 2 GB RAM                     |
| Hard Disk       | 100 GB (Single Virtual Disk) |
| Firmware        | UEFI                         |
| Network Adapter | NAT                          |
| CD/DVD Drive    | Windows 10 ISO               |
| USB Controller  | Default                      |
| Sound Card      | Default                      |
| Printer         | Disabled (Optional)          |

Review the hardware settings and verify that the Windows 10 ISO is attached before powering on the virtual machine.

> **📸 Screenshot Required**
>
> **Title:** Windows 10 Virtual Machine Hardware
>
> **Save As:** `screenshots/windows/03-vm-hardware-settings.png`

---

## Optional VMware Settings

The following virtualization options were left **unchecked** during the creation of the Windows 10 virtual machine because they are not required for this Enterprise SOC Lab.

| VMware Option | Status | Reason |
|---------------|--------|--------|
| Virtualize Intel VT-x/EPT or AMD-V/RVI | Unchecked | Nested virtualization is not required for the Windows 10 endpoint. |
| Virtualize CPU Performance Counters | Unchecked | Performance counter virtualization is unnecessary for this lab. |
| Virtualize IOMMU (IO Memory Management Unit) | Unchecked | PCI device passthrough and advanced virtualization features are not used in this project. |

These settings can remain at their default values without affecting the functionality of the Enterprise SOC Lab.

> **📸 Screenshot Required**
>
> **Title:** VMware Virtualization Engine Settings
>
> **Save As:** `screenshots/windows/04-virtualization-engine-settings.png`

---

# Install Windows 10

Power on the virtual machine and boot from the Windows 10 ISO.

The Windows Setup wizard will guide you through the installation process. Select the recommended options shown below.

## Installation Method

The Enterprise SOC Lab was initially developed using a pre-configured Windows 10 virtual machine.

For this documentation, the installation process is demonstrated using the official Microsoft Windows 10 ISO. This approach ensures that readers can recreate the lab from a clean installation without relying on pre-built virtual machines.

## Installation Configuration

| Configuration            | Recommended Selection                         |
| ------------------------ | --------------------------------------------- |
| Language to Install      | English (United States)                       |
| Time and Currency Format | English (United States)                       |
| Keyboard Layout          | US                                            |
| Install Now              | Continue                                      |
| Product Key              | Enter a valid product key or select "I don't have a product key" if available. |
| Windows Edition          | Windows 10 Pro                                |
| License Terms            | Accept                                        |
| Installation Type        | Custom: Install Windows Only (Advanced)       |
| Installation Drive       | Primary Virtual Disk                          |
| Disk Partition           | Use Entire Disk (Default)                     |

After selecting the installation drive, Windows Setup will copy the installation files and restart the virtual machine automatically several times.

Wait until the Out-of-Box Experience (OOBE) appears before continuing with the initial Windows configuration.

> **📸 Screenshot Required**
>
> **Title:** Windows Setup Wizard
>
> **Save As:** `screenshots/windows/05-windows-setup-wizard.png`

> **📸 Screenshot Required**
>
> **Title:** Windows Installation Progress
>
> **Save As:** `screenshots/windows/06-installation-progress.png`

# Configure Windows 10

After the installation files have been copied and the system restarts, Windows will launch the Out-of-Box Experience (OOBE). Complete the initial configuration using the recommended settings below.

---

## Region

Select the appropriate region for the operating system.

| Setting | Recommended Value   |
| ------- | ------------------- |
| Region  | Your Country/Region |

> **📸 Screenshot Required**
>
> **Title:** Region Selection
>
> **Save As:** `screenshots/windows/07-region-selection.png`

---

## Keyboard Layout

Select the primary keyboard layout.

| Setting         | Recommended Value |
| --------------- | ----------------- |
| Keyboard Layout | US                |

If prompted to add a second keyboard layout, select **Skip** unless an additional layout is required.

> **📸 Screenshot Required**
>
> **Title:** Keyboard Layout Selection
>
> **Save As:** `screenshots/windows/08-keyboard-layout.png`

---

## Local User Account

Create a local administrator account for managing the virtual machine.

| Setting      | Recommended Value       |
| ------------ | ----------------------- |
| Account Type | Local Account           |
| Username     | Your Preferred Username |

Using a local account keeps the lab environment independent of a Microsoft account and simplifies administration.

> **📸 Screenshot Required**
>
> **Title:** Local User Account
>
> **Save As:** `screenshots/windows/09-local-user-account.png`

---

## Password

Configure a strong password for the local user account.

| Setting          | Recommendation  |
| ---------------- | --------------- |
| Password         | Strong Password |
| Confirm Password | Same as Above   |

A strong password should include:

* Uppercase letters
* Lowercase letters
* Numbers
* Special characters

> **📸 Screenshot Required**
>
> **Title:** Password Configuration
>
> **Save As:** `screenshots/windows/10-password-configuration.png`

---

## Privacy Settings

Review the Windows privacy options before completing the setup.

For a lab environment, the default settings are generally sufficient. You may adjust them later if required.

> **📸 Screenshot Required**
>
> **Title:** Privacy Settings
>
> **Save As:** `screenshots/windows/11-privacy-settings.png`

---

# Verify the Installation

After completing the initial setup, sign in using the local user account and verify that Windows is functioning correctly.

## Verification Commands

Open **Command Prompt** and run the following commands.

```cmd
hostname
```

```cmd
whoami
```

```cmd
ipconfig
```

```cmd
systeminfo
```

```cmd
ping google.com
```

## Expected Results

| Verification          | Expected Result                               |
| --------------------- | --------------------------------------------- |
| Login                 | Successful login using the local user account |
| Computer Name         | Automatically Assigned Computer Name         |
| User Account          | Displays the logged-in user                   |
| Operating System      | Windows 10 Pro (64-bit) is displayed          |
| Network               | A valid IPv4 address is assigned              |
| Internet Connectivity | Ping receives successful replies              |

> **📸 Screenshot Required**
>
> **Title:** Windows First Login
>
> **Save As:** `screenshots/windows/12-first-login.png`

> **📸 Screenshot Required**
>
> **Title:** Windows Installation Verification
>
> **Save As:** `screenshots/windows/13-installation-verification.png`

---

# Update Windows 10

After verifying the installation, install the latest Windows updates before deploying additional software.

Navigate to:

```text
Settings → Update & Security → Windows Update
```

Select **Check for updates**, install all available updates, and restart the virtual machine if prompted.

## Verification

Confirm the following after the update process completes:

* Windows Update completed successfully.
* The system restarted without errors.
* Login is successful.
* Internet connectivity is available.

> **📸 Screenshot Required**
>
> **Title:** Windows Update
>
> **Save As:** `screenshots/windows/14-windows-update.png`

# Installation Complete

Windows 10 has now been successfully installed, configured, verified, and updated.

The operating system is ready to function as the monitored endpoint in the Enterprise SOC Lab. In the next chapters, this endpoint will be prepared for security monitoring by installing the required security components.

Before proceeding, complete the installation checklist below.

---

# Installation Checklist

Verify that each task has been completed successfully.

| Status | Task                                 |
| ------ | ------------------------------------ |
| ☐      | Windows 10 installed successfully    |
| ☐      | Virtual machine boots without errors |
| ☐      | Local user account created           |
| ☐      | Login verified                       |
| ☐      | Computer name configured             |
| ☐      | Network connectivity verified        |
| ☐      | Internet access confirmed            |
| ☐      | Windows Update completed             |
| ☐      | System restarted successfully        |

---

# Common Installation Issues

| Issue                            | Possible Cause                                               | Resolution                                                                                    |
| -------------------------------- | ------------------------------------------------------------ | --------------------------------------------------------------------------------------------- |
| Virtual machine does not boot    | ISO not attached or incorrect boot order                     | Verify that the Windows 10 ISO is attached and the boot order is correct.                     |
| Windows Setup does not start     | Virtual machine booted from the hard disk instead of the ISO | Restart the virtual machine and boot from the Windows 10 ISO.                                 |
| Unable to create a local account | Microsoft account prompt                                     | Select **Offline Account** or **Domain Join Instead** if available to create a local account. |
| No IP address assigned           | Network adapter configuration                                | Verify that the VMware network adapter is enabled and configured for NAT.                     |
| No internet connectivity         | VMware networking or host network issue                      | Confirm the host has internet access and verify the VMware NAT configuration.                 |
| Windows Update fails             | Temporary network or update service issue                    | Restart the virtual machine and run **Windows Update** again.                                 |
| Slow virtual machine performance | Insufficient CPU, memory, or disk resources                  | Increase the allocated virtual hardware if the host system has available resources.           |

---

# Summary

In this chapter, you installed Windows 10 for the Enterprise SOC Lab.

The virtual machine was created, hardware resources were configured, Windows 10 was installed, the initial operating system configuration was completed, the installation was verified, and the latest Windows updates were installed. The endpoint is now ready for the deployment of security monitoring components.

---

# Key Takeaways

After completing this chapter, you can:

* Download the Windows 10 ISO from the official Microsoft website.
* Create a Windows 10 virtual machine in VMware Workstation.
* Configure the recommended virtual hardware.
* Install Windows 10 using the recommended settings.
* Perform the initial Windows configuration.
* Verify that the operating system is functioning correctly.
* Update Windows 10 before installing additional software.

---

# Next Chapter

The next chapter focuses on installing Kali Linux, which will be used as the attacker machine throughout the Enterprise SOC Lab.

Kali Linux will simulate real-world attacks against the Windows 10 endpoint, allowing the Wazuh platform to collect, detect, and analyze security events generated during attack simulations.

**Topics Covered**

* Downloading the Kali Linux ISO
* Creating the Kali Linux virtual machine
* Configuring the virtual hardware
* Installing Kali Linux
* Performing the initial system configuration
* Verifying network connectivity
* Updating the operating system

**Next Document**

`06 - Kali Linux Installation.md`

---

# Document Revision History

| Version | Date      | Author       | Description                                           |
| ------- | --------- | ------------ | ----------------------------------------------------- |
| 1.0     | July 2026 | Rajeev Kumar | Initial release of the Windows 10 Installation guide. |
