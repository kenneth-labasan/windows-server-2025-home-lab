# Module 16 - Windows Server PowerShell Administration
> **Module Overview**
>
> This module introduces Windows PowerShell as a command-line administration tool for Windows Server. It demonstrates how to retrieve information from Active Directory, DHCP, DNS, and Group Policy, as well as generate administrative reports using PowerShell. The module reinforces PowerShell as an essential skill for Windows Server administration and automation.

# Objective
Use Windows PowerShell to administer and retrieve information from a Windows Server environment without relying solely on graphical management tools.

# Skills Demonstrated
- Windows PowerShell administration
- Active Directory management using PowerShell
- DHCP administration using PowerShell
- DNS administration using PowerShell
- Group Policy administration using PowerShell
- PowerShell pipelines
- Administrative reporting
- CSV export

# Technologies Used
| Technology | Purpose |
|------------|---------|
| Windows PowerShell | Command-line administration |
| Windows Server 2025 | Server operating system |
| Active Directory Module | Directory administration |
| DHCP Server Module | DHCP administration |
| DNS Server Module | DNS administration |
| Group Policy Module | Group Policy management |

# Lab Environment
| Component | Value |
|-----------|-------|
| Server | DC01 |
| Domain | kennethlab.test |
| Operating System | Windows Server 2025 |

# Prerequisites
Completed Modules:

- Module 12 – Active Directory Users and Group Management
- Module 13 – Shared Folder and NTFS Permissions
- Module 14 – Group Policy Management
- Module 15 – Dynamic Host Configuration Protocol (DHCP)

# Architecture Overview
```text
                  Windows Server 2025
                          │
                   Windows PowerShell
                          │
      ┌───────────────────┼────────────────────┐
      │                   │                    │
Active Directory      DHCP Server        DNS Server
      │                   │                    │
      └───────────────────┼────────────────────┘
                          │
                   Group Policy
                          │
                  Administrative Reports
```

# Procedure
## Step 1 - Verify Windows PowerShell
Opened Windows PowerShell as Administrator.

Executed:

```powershell
$PSVersionTable
```

Verified:

- PowerShell Version
- Edition
- Platform
- Operating System

## Step 2 - Discover Available Commands
Executed:

```powershell
Get-Command
```

Filtered Active Directory cmdlets:

```powershell
Get-Command *AD*
```

Reviewed command documentation:

```powershell
Get-Help Get-ADUser

Get-Help Get-ADUser -Examples
```

## Step 3 - Retrieve Active Directory Information
Retrieved all Active Directory users:

```powershell
Get-ADUser -Filter *
```

Displayed selected properties:

```powershell
Get-ADUser -Filter * |
Select-Object Name, SamAccountName, Enabled
```

Listed Organizational Units:

```powershell
Get-ADOrganizationalUnit -Filter * |
Select-Object Name, DistinguishedName
```

Listed Security Groups:

```powershell
Get-ADGroup -Filter * |
Select-Object Name, GroupCategory, GroupScope
```

Displayed group membership:

```powershell
Get-ADGroupMember HR_Users

Get-ADGroupMember IT_Users
```

Retrieved detailed information for Jane Smith:

```powershell
Get-ADUser jane.smith -Properties * |
Select-Object Name, Enabled, Department, LastLogonDate
```

Counted Active Directory user objects:

```powershell
(Get-ADUser -Filter *).Count
```

## Step 4 - Retrieve Windows Server Role Information
Retrieved DHCP configuration:

```powershell
Get-DhcpServerv4Scope

Get-DhcpServerv4Lease -ScopeId 192.168.0.0
```

Retrieved DNS configuration:

```powershell
Get-DnsServerZone

Get-DnsServerResourceRecord -ZoneName "kennethlab.test"
```

Retrieved Group Policy information:

```powershell
Get-GPO -All

gpresult /r
```

## Step 5 - Generate Administrative Report
Created a report directory:

```powershell
New-Item -ItemType Directory -Path C:\Reports
```

Exported Active Directory users:

```powershell
Get-ADUser -Filter * |
Select-Object Name, SamAccountName, Enabled |
Export-Csv C:\Reports\ADUsers.csv -NoTypeInformation
```

Verified that the CSV report was successfully generated.

# Verification
Verified that:

- Windows PowerShell launched successfully.
- Active Directory cmdlets were available.
- Organizational Units were retrieved.
- Security Groups and memberships were retrieved.
- DHCP scopes and leases were displayed.
- DNS zones and resource records were displayed.
- Group Policy Objects were displayed.
- Group Policy Results were generated.
- Active Directory information was successfully exported to CSV.

# Lessons Learned
- Windows PowerShell provides a centralized interface for administering multiple Windows Server roles.
- PowerShell pipelines simplify filtering and formatting command output.
- `Get-Help` is an essential resource for learning PowerShell cmdlets.
- `Export-Csv` is an effective method for generating administrative reports.
- Verifying system resources before using file paths prevents unnecessary execution errors.

# Engineering Notes
PowerShell significantly improves administrative efficiency by allowing multiple Windows Server roles to be managed from a single interface. Instead of opening several graphical management consoles, administrators can retrieve, filter, and export information using concise and repeatable commands.

This approach becomes increasingly valuable as environments grow larger and administrative tasks become more repetitive.

# Best Practices
- Run PowerShell with administrative privileges when managing Windows Server roles.
- Use `Get-Help` before executing unfamiliar cmdlets.
- Display only the required properties using `Select-Object`.
- Export administrative reports rather than manually copying information.
- Validate command output after each administrative task.

# Key Takeaways
- Verified the Windows PowerShell environment.
- Learned how to discover PowerShell cmdlets.
- Retrieved Active Directory information using PowerShell.
- Retrieved DHCP configuration using PowerShell.
- Retrieved DNS configuration using PowerShell.
- Retrieved Group Policy information using PowerShell.
- Generated an administrative CSV report.
- Applied PowerShell as a centralized Windows Server administration tool.

# Summary
In this module, Windows PowerShell was used to administer a Windows Server environment by retrieving information from Active Directory, DHCP, DNS, and Group Policy. Administrative reports were generated by exporting Active Directory user information to a CSV file. This module demonstrated how PowerShell provides an efficient, repeatable, and scalable approach to Windows Server administration while reducing dependence on graphical management tools.

# Next Module
**Module 17 – Windows Server Troubleshooting Scenarios**