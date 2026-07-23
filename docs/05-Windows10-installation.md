# Windows 10 Installation (VMware Image)

---

# Chapter Information

| Property | Details |
|----------|---------|
| **Chapter Number** | 05 |
| **Chapter Title** | Windows 10 Installation |
| **Difficulty** | Beginner |
| **Estimated Reading Time** | 10–15 Minutes |
| **Estimated Lab Time** | 15–25 Minutes |
| **Prerequisite** | Chapter 04 – Ubuntu Server Installation |
| **Category** | Installation Guide |
| **Related Troubleshooting** | Troubleshooting.md |
| **Next Chapter** | Chapter 06 – Sysmon Installation |

---

# Document Information

| Property | Details |
|----------|---------|
| **Project** | Enterprise SOC Lab |
| **Document Name** | Windows 10 Installation |
| **Version** | 1.0 |
| **Author** | Rajeev Rock |
| **Status** | Published |
| **Last Updated** | July 2026 |

---

# Learning Objectives

After completing this chapter, you will be able to:

- Download the official Windows 10 VMware virtual machine.
- Import the virtual machine into VMware Workstation Pro.
- Configure the recommended virtual hardware.
- Verify a successful startup and login.
- Update Windows before installing security tools.

---

# Objective

This chapter explains how to prepare the Windows 10 endpoint used throughout the Enterprise SOC Lab.

Instead of installing Windows from an ISO image, this project uses the official Microsoft Windows 10 Enterprise Evaluation virtual machine. This approach reduces setup time while providing a fully functional endpoint for security monitoring and investigation.

After completing this chapter, the Windows endpoint will be ready for Sysmon installation and Wazuh Agent deployment.

---

# Download Windows 10 VMware Image

Download the official Windows 10 Enterprise Evaluation virtual machine from Microsoft's Developer website.

Using the official virtual machine ensures that the operating system is authentic, up to date, and ready for VMware Workstation.

## Download Requirements

| Component | Recommended Value |
|----------|-------------------|
| Edition | Windows 10 Enterprise Evaluation |
| Platform | VMware |
| Architecture | 64-bit |
| File Format | ZIP Archive |

After downloading, extract the archive to your project directory.

Example:

```text
Enterprise-SOC-Lab/
│
├── Images/
│   ├── Ubuntu-Server.iso
│   ├── Windows10-VMware.zip
│   └── Kali-Linux-VMware.7z
│
├── VMware/
├── docs/
└── screenshots/
```

# Import the Virtual Machine

After extracting the downloaded archive, import the virtual machine into VMware Workstation Pro.

Open VMware Workstation and select **Open a Virtual Machine**, then browse to the extracted folder and open the **`.vmx`** file.

Example files:

```text
Windows 10 x64.vmx
Windows 10 x64.vmdk
Windows 10 x64.nvram
```

Once the virtual machine appears in VMware Workstation, verify that it loads without any errors before powering it on.

> **📸 Screenshot Required**
>
> **Title:** Windows 10 VMware Files (.vmx)
>
> **Save As:** `screenshots/windows/01-vm-files.png`

---

# Configure Virtual Machine Hardware

Before starting the virtual machine, review the hardware configuration.

## Recommended Hardware Configuration

| Component | Configuration |
|----------|---------------|
| Processors | 2 vCPUs |
| Memory | 4 GB RAM |
| Hard Disk | 60 GB |
| Network Adapter | NAT |
| CD/DVD Drive | Default |
| USB Controller | Default |

Review the settings and ensure that the network adapter is configured to **NAT**. If your host system has additional available resources, you may increase the virtual machine memory later based on your requirements.

> **📸 Screenshot Required**
>
> **Title:** Windows 10 Virtual Machine Hardware Settings
>
> **Save As:** `screenshots/windows/02-hardware-settings.png`

# First Boot and Login

Power on the virtual machine and sign in using the default credentials provided with the Microsoft Evaluation virtual machine.

After logging in, verify the following:

- Windows starts successfully.
- The desktop loads without errors.
- A valid IP address is assigned.
- Internet connectivity is available.

> **📸 Screenshot Required**
>
> **Title:** Windows 10 First Login
>
> **Save As:** `screenshots/windows/03-first-login.png`

---

# Update Windows

Before installing Sysmon and the Wazuh Agent, install the latest available Windows updates.

Navigate to:

**Settings → Windows Update**

Install all available updates and restart the virtual machine if prompted.

Keeping Windows updated improves security, stability, and compatibility with the tools used throughout this project.

---

# Installation Complete

The Windows 10 endpoint has now been successfully imported, configured, and updated.

It is ready for endpoint monitoring and will be used throughout the Enterprise SOC Lab for generating security events and performing investigations.

---

# Installation Checklist

Verify that each task has been completed successfully before proceeding to the next chapter.

| Status | Task |
|--------|------|
| ☐ | Windows 10 VMware image downloaded |
| ☐ | Virtual machine imported successfully |
| ☐ | Hardware configuration verified |
| ☐ | Windows booted successfully |
| ☐ | Login verified |
| ☐ | Network connectivity confirmed |
| ☐ | Windows updated |

---

# Summary

In this chapter, the Windows 10 VMware virtual machine was downloaded, imported into VMware Workstation Pro, configured with the recommended hardware settings, verified, and updated.

The endpoint is now ready for Sysmon installation and Wazuh Agent deployment.

---

# Key Takeaways

After completing this chapter, you can:

- Import a Windows 10 VMware virtual machine.
- Configure the recommended virtual hardware.
- Verify a successful installation.
- Prepare the endpoint for security monitoring.

---

# Next Chapter

The next chapter covers the installation and configuration of **Sysmon**, which provides detailed Windows endpoint telemetry for security monitoring and analysis.

**Next Document:** `06 - Sysmon Installation.md`

---

# Document Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | July 2026 | Rajeev Rock | Initial release of the Windows 10 VMware Installation guide. |