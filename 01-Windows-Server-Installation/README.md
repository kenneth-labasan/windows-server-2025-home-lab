# Module 1 - Windows Server 2025 Installation

## Objective

Install Windows Server 2025 on the physical server and prepare a stable operating system that will later serve as the Hyper-V host for my home lab.

This module also documents the preparation of the installation media, storage configuration, and the troubleshooting steps I encountered during installation.

---

# Skills Demonstrated

- Windows Server deployment
- Bootable USB creation
- BIOS troubleshooting
- Storage preparation
- Disk partition planning
- Windows Update
- Technical troubleshooting
- Documentation

---

# Technologies Used

- Windows Server 2025
- Rufus Portable
- DiskPart
- BIOS (UEFI)
- NVMe SSD

# Lab Environment

## Hardware
**Component**       **Specification**
Processor           Intel Core i5-10500T
Memory              32 GB RAM
Storage             1 TB NVMe SSD
Future Host Name    HV01

# Background

Before Windows Server roles such as Hyper-V, Active Directory Domain Services, DNS, and DHCP can be installed, the operating system must first be deployed correctly.

A properly prepared installation reduces future configuration problems and provides a stable foundation for the lab environment.

# Procedure

## Step 1 - Download Windows Server ISO

Downloaded the official Windows Server 2025 ISO that will be used to create the installation media.

### Screenshot

insert screenshot

## Step 2 - Create Bootable USB

### Purpose

Create a bootable USB drive containing the Windows Server 2025 installation media.

### Actions Performed

- Downloaded the Windows Server 2025 ISO.
- Used Rufus to create a bootable USB installer.
- Prepared the USB drive for Windows Server installation.

### My Experience

During a previous Windows 11 installation on my companion's desktop computer, I chose **Rufus Portable** because I assumed it was the better version.

During that installation, I encountered several issues and became confused about the installation process. At one point, after removing the installation USB drive, Windows no longer booted correctly. Looking back, I realized I had made mistakes during the installation and boot process rather than the issue being caused by Windows itself.

After reviewing the differences between the two editions of Rufus, I realized that I had misunderstood the purpose of the Portable version.

### What I Learned

For my personal workflow, **Rufus Standard** is the better choice because I create bootable USB drives on my own computer and do not need Rufus to carry its settings between multiple computers.

I learned that:

- **Rufus Standard** is the recommended edition for typical Windows installation tasks.
- **Rufus Portable** is designed for users who want to carry Rufus between different computers while preserving its configuration.

The difference is based on portability and configuration storage, **not** on installation performance.

### Reflection

While reviewing my previous Windows 11 installation, I also realized that some of the installation problems were likely caused by my own installation process rather than by Rufus itself.

One possibility is that I did not properly verify the boot device during installation. This experience reminded me that understanding the installation workflow is just as important as selecting the correct tool.

### Lesson Learned

This experience taught me that troubleshooting should begin by reviewing my own installation steps before assuming the software or hardware is at fault.

It also reinforced the importance of understanding the purpose of the tools I use instead of choosing one based on assumptions.

## Step 3 - Boot from the USB Installer

Configured the computer to boot from the Windows Server installation media.

During the installation process, Windows Setup could not detect the NVMe SSD.

## Step 4 - Troubleshoot Storage Detection

### Problem

Windows Setup did not display any available storage devices.

### Investigation

Instead of assuming the SSD was faulty, I investigated the BIOS configuration.

I discovered that the Intel VMD Controller was enabled.

### Resolution

- Disabled Intel VMD Controller.
- SATA Controller Mode became available.
- Changed the storage controller mode to AHCI.
- Restarted the installation.

After changing the BIOS configuration, Windows Setup successfully detected the SSD.

### Lesson

This experience taught me that storage detection issues are not always hardware failures. BIOS storage controller settings can prevent Windows Setup from detecting NVMe drives.

## Step 5 - Install Windows Server

Installed Windows Server 2025 using the detected NVMe SSD.

The installation completed successfully.

> Note:
> I do not have screenshots of the installation wizard because screenshots cannot be captured during the operating system installation process.

## Step 6 - Partition the Storage

After Windows Server installation, I organized the storage into three logical partitions.

| Drive | Label         |  Purpose                                                                |
|-------|---------------|-------------------------------------------------------------------------|
| C:    | System        | Windows Server operating system                                         |
| D:    | Lab           | Hyper-V virtual machines and lab files                                  |
| E:    | Resources     | ISO files, drivers, scripts, installers, screenshots, and documentation |

This storage layout keeps the operating system separate from lab resources and makes future maintenance easier.

---

## Step 7 - Install Windows Updates

After completing the installation, I installed the latest Windows Updates before proceeding with any server roles.

Keeping the operating system updated provides a more secure and stable foundation for future configuration.

### Screenshot

insert screenshot

---

# Verification

The installation was considered successful after verifying the following:

- Windows Server booted successfully.
- Windows detected the NVMe SSD correctly.
- The storage partitions were created successfully.
- Internet connectivity was available.
- Windows Update completed successfully.

# Problems Encountered

## Problem

Windows Setup could not detect the NVMe SSD.

### Cause

Intel VMD Controller was enabled in the BIOS.

### Resolution

Disabled Intel VMD Controller and switched the storage controller mode to AHCI.

### Result

Windows Setup successfully detected the SSD and the installation proceeded normally.


# Lessons Learned

During this module, I learned several practical lessons beyond simply installing Windows Server.

### Difference Between Rufus Standard and Rufus Portable

I learned that Rufus Portable stores its configuration inside the application folder, making it convenient to use across different computers.

---

### BIOS Configuration Can Affect Windows Installation

I learned that BIOS settings can directly affect whether Windows Setup detects storage devices.

Rather than assuming the SSD was defective, I investigated the BIOS configuration first.

---

### Intel VMD Controller

Before this installation, I was unfamiliar with Intel VMD.

After troubleshooting, I learned that Intel VMD is designed for specific storage configurations but may require additional drivers during Windows installation.

For my lab environment, disabling Intel VMD and using AHCI allowed Windows Setup to recognize the NVMe SSD.

---

### Storage Organization

Instead of placing everything on the system drive, I organized the storage into dedicated partitions for the operating system, virtual machines, and lab resources.

This will simplify future Hyper-V management and improve organization.

---

### Build First, Configure Later

I learned that installing Windows Server is only the first stage of building a server.

Proper planning before installing server roles helps reduce rework later.

---

# Best Practices

- Download installation media from Microsoft's official source.
- Keep Windows updated before installing server roles.
- Separate operating system files from virtual machine storage.
- Investigate BIOS settings before assuming hardware failure.
- Document problems and their solutions for future reference.

---

# Real-World Relevance

System administrators are responsible not only for installing operating systems but also for troubleshooting hardware detection issues, preparing storage, and ensuring servers are stable before deploying production services.

The troubleshooting experience in this module reflects a common real-world deployment scenario where installation problems are caused by firmware or BIOS configuration rather than faulty hardware.

---

# Summary

In this module, I successfully installed Windows Server 2025 on my physical server, resolved a storage detection issue caused by the Intel VMD Controller, organized the storage into dedicated partitions, and updated the operating system.

This completed the foundation required for building the Hyper-V home lab.

---

# Next Module

Module 2 - Hyper-V Host Preparation