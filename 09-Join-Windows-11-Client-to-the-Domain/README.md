# Module 10 - Install Windows 11 on WIN11-01
> **Module Overview**
>
> This module covers the installation of Windows 11 Pro on the WIN11-01 virtual machine. The installation included the Windows Setup process, Out-of-Box Experience (OOBE), initial operating system configuration, Windows Update, and verification of the client operating system before preparing it for the Active Directory environment.

# Objective
Install and prepare Windows 11 as the client operating system for the Windows Server home lab.

# Skills Demonstrated
- Windows 11 installation
- Hyper-V virtual machine deployment
- Windows Out-of-Box Experience (OOBE)
- Windows Update
- Operating system verification
- Initial client configuration
- Technical documentation

# Technologies Used
| Technology | Purpose |
|------------|---------|
| Windows 11 Pro | Client operating system |
| Hyper-V | Virtualization platform |
| Windows Update | Operating system updates |
| Microsoft Account | Temporary account used to complete Windows 11 OOBE |

# Lab Environment
| Component | Value |
|-----------|-------|
| Virtual Machine | WIN11-01 |
| Operating System | Windows 11 Pro |
| Generation | Generation 2 |
| Startup Memory | 4096 MB (Dynamic Memory Enabled) |
| Processor | 2 Virtual Processors |
| Virtual Hard Disk | 80 GB VHDX |
| Virtual Switch | External vSwitch |

# Prerequisites
Before beginning this module, I completed:

- Created the WIN11-01 virtual machine
- Attached the Windows 11 ISO
- Enabled Secure Boot
- Enabled Trusted Platform Module (TPM)
- Configured the virtual hardware

# Procedure
## Step 1 - Configure Windows Setup
### Purpose
Begin the Windows 11 installation.

### Actions Performed
Configured:

- Language
- Time and currency format
- Keyboard layout

Selected **Install Windows** to begin the installation.

![Windows Setup](images/01-windows-setup.PNG)

*Figure 10.1. Windows Setup configuration.*

## Step 2 - Product Key
### Purpose
Continue the installation without product activation.

### Actions Performed
Selected:

```
I don't have a product key
```

This allows Windows to be installed before activation.

![Product Key](images/02-product-key.PNG)

*Figure 10.2. Continuing the installation without entering a product key.*

## Step 3 - Select Windows Edition
### Purpose
Choose the Windows edition required for Active Directory.

### Actions Performed
Selected:

```
Windows 11 Pro
```

### Why Windows 11 Pro?
Windows 11 Pro supports:

- Active Directory domain join
- Group Policy
- Business management features

Windows 11 Home does not support joining an on-premises Active Directory domain.

![Edition Selection](images/03-edition.PNG)

*Figure 10.3. Selecting Windows 11 Pro.*

## Step 4 - Accept the License Agreement
Accepted the Microsoft Software License Terms and continued with the installation.

![License Agreement](images/04-license.PNG)

*Figure 10.4. Accepting the license agreement.*

## Step 5 - Select Installation Drive
### Actions Performed
Selected:

```
Drive 0 Unallocated Space
```

Windows automatically created the required system partitions and began installing Windows.

![Disk Selection](images/05-disk.PNG)

*Figure 10.5. Selecting the installation drive.*

## Step 6 - Install Windows
Windows Setup completed the following tasks:

- Copying Windows files
- Installing features
- Installing updates
- Completing the installation

The virtual machine restarted automatically several times.

![Installing Windows](images/06-installing.PNG)

*Figure 10.6. Windows installation in progress.*

## Step 7 - Complete the Out-of-Box Experience (OOBE)
### Actions Performed
Configured:

- Region
- Keyboard layout
- Device name

Assigned the computer name:

```
WIN11-01
```

Selected:

```
Set up as a new PC
```

to create a clean Windows installation.

![OOBE](images/07-oobe.PNG)

*Figure 10.7. Windows 11 Out-of-Box Experience.*

## Step 8 - Microsoft Account
### Actions Performed
During Windows 11 setup, the operating system required a Microsoft account to continue.

I signed in using a temporary Microsoft account to complete the installation.

This account will not affect joining the computer to the on-premises Active Directory domain later in the lab.

![Microsoft Account](images/08-microsoft-account.PNG)

*Figure 10.8. Microsoft account sign-in during OOBE.*

## Step 9 - Optional Microsoft Services
To keep the virtual machine focused on the Active Directory lab, I declined optional Microsoft consumer services, including:

- Microsoft 365 Personal
- Microsoft 365 Basic
- Browser history personalization
- Other optional Microsoft offers presented during setup

## Step 10 - Windows Update
### Purpose
Update the operating system before joining the Active Directory domain.

### Actions Performed
Opened:

```
Settings
→ Windows Update
```

Installed all available updates.

Restarted the virtual machine when required.

Verified that Windows Update reported:

```
You're up to date
```

![Windows Update](images/09-windows-update.PNG)

*Figure 10.9. Updating Windows 11.*

## Step 11 - Verify Installation
### Verification
Confirmed:

- Windows installation completed successfully.
- Windows 11 Pro installed.
- Computer name is WIN11-01.
- Windows Update completed successfully.
- The client operating system is ready for Active Directory configuration.

![System About](images/10-about.PNG)

*Figure 10.10. Windows 11 installation verification.*

# Verification
I confirmed that:

- Windows 11 Pro installed successfully.
- WIN11-01 booted normally.
- Windows Update completed successfully.
- The operating system is ready to join the Active Directory domain.

# Problems Encountered
## Windows 11 Required a Microsoft Account

During the Out-of-Box Experience, Windows 11 (25H2) required both an Internet connection and a Microsoft account before allowing the installation to continue.

Initially, I attempted to complete the installation without an Internet connection using the Internal virtual switch, but Windows stopped at the **"Let's connect you to a network"** screen and would not proceed.

After switching to the External virtual switch, Windows was able to connect to the Internet and continue the setup.

To complete the installation, I signed in using a temporary Microsoft account.

This does not prevent the computer from joining the on-premises Active Directory domain in a later module.

# Lessons Learned
- Windows 11 Pro is required for joining an on-premises Active Directory domain.
- Generation 2 virtual machines are recommended for Windows 11 because they support Secure Boot and TPM.
- Recent Windows 11 releases (25H2) require an Internet connection and Microsoft account during OOBE.
- Installing Windows updates before domain configuration provides a stable client operating system.

# Best Practices
- Install all Windows updates before joining the domain.
- Use Windows 11 Pro for Active Directory environments.
- Keep the client operating system fully updated before applying Group Policy.
- Use a consistent computer naming convention.

# Summary
In this module, I successfully installed Windows 11 Pro on the WIN11-01 virtual machine, completed the Windows Out-of-Box Experience, signed in with a temporary Microsoft account due to Windows 11 25H2 requirements, updated the operating system, and verified that the client is ready for Active Directory configuration.

# Next Module
**Module 11 - Configure Windows 11 Client and Join the Active Directory Domain**

The next module will configure the client's network settings, verify DNS connectivity, and join WIN11-01 to the **kennethlab.test** Active Directory domain.