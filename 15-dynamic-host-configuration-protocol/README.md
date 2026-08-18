# Module 15 - Dynamic Host Configuration Protocol (DHCP)
> **Module Overview**
>
> This module covers the installation, authorization, and configuration of the Dynamic Host Configuration Protocol (DHCP) Server role. It also demonstrates creating a DHCP scope, configuring scope options, and validating automatic IP address assignment on a Windows 11 domain client.

# Objective
Install and configure a DHCP Server that automatically assigns IP addresses and network settings to domain clients within the Active Directory environment.

# Skills Demonstrated
- DHCP Server installation
- DHCP Server authorization
- DHCP scope configuration
- IP address management
- DHCP exclusions
- DHCP options configuration
- Client IP lease validation
- Network troubleshooting
- Technical documentation

# Technologies Used
| Technology | Purpose |
|------------|---------|
| Windows Server 2025 | DHCP Server |
| Active Directory Domain Services | DHCP authorization |
| DHCP | Automatic IP address assignment |
| Windows 11 Pro | DHCP client |
| Command Prompt | DHCP verification |

# Lab Environment
| Component | Value |
|-----------|-------|
| DHCP Server | DC01 |
| Client Computer | WIN11-01 |
| Domain | kennethlab.test |
| Network | 192.168.0.0/24 |
| DHCP Scope | Office LAN |

# Prerequisites
Before beginning this module, I completed:

- Module 8 – Configure Domain Name System (DNS)
- Module 11 – Configure Windows 11 Client and Join the Active Directory Domain
- Module 14 – Group Policy Management

# Procedure
## Step 1 - Install the DHCP Server Role
### Purpose
Install the DHCP Server role on DC01.

### Actions Performed
Opened:

```
Server Manager
→ Add Roles and Features
```

Selected:

```
DHCP Server
```

Installed the required features and completed the installation.

![Install DHCP Role](images/01-install-dhcp-role.png)

*Figure 15.1. Installing the DHCP Server role.*

## Step 2 - Authorize the DHCP Server
### Purpose
Authorize the DHCP Server within Active Directory.

### Actions Performed
Opened the Server Manager notification.

Selected:

```
Complete DHCP configuration
```

Used the Domain Administrator credentials.

Completed:

- DHCP authorization
- Security group creation

Verified that the post-deployment configuration completed successfully.

![Authorize DHCP](images/02-authorize-dhcp.png)

*Figure 15.2. Completing DHCP post-deployment configuration.*

## Step 3 - Create a DHCP Scope
### Purpose
Create an IPv4 scope for automatic IP address assignment.

### Actions Performed
Opened:

```
Server Manager
→ Tools
→ DHCP
```

Created a new IPv4 scope with the following configuration:

| Setting | Value |
|---------|-------|
| Scope Name | Office LAN |
| Description | DHCP Scope for Home Lab |
| Start IP Address | 192.168.0.100 |
| End IP Address | 192.168.0.200 |
| Subnet Mask | 255.255.255.0 |

Configured an exclusion:

| Start | End |
|-------|-----|
| 192.168.0.110 | 192.168.0.110 |

This exclusion prevented DHCP from assigning the IP address that was previously configured as a static address on WIN11-01.

![Create Scope](images/03-create-scope.png)

*Figure 15.3. Creating the DHCP scope.*

## Step 4 - Configure DHCP Options
### Purpose
Provide clients with the required network configuration.

### Actions Performed
Configured:

| Option | Value |
|--------|-------|
| Default Gateway | 192.168.0.1 |
| DNS Server | 192.168.0.10 |
| Parent Domain | kennethlab.test |
| Lease Duration | 8 Days |

Activated the DHCP scope.

![Configure DHCP Options](images/04-dhcp-options.png)

*Figure 15.4. Configuring DHCP scope options.*

## Step 5 - Validate DHCP Operation
### Purpose
Verify that clients receive network configuration automatically.

### Actions Performed
On WIN11-01:

Changed the network adapter from:

```
Manual
```

to:

```
Automatic (DHCP)
```

Executed:

```cmd
ipconfig /release
ipconfig /renew
ipconfig /all
```

Verified the client received:

| Setting | Value |
|---------|-------|
| IPv4 Address | 192.168.0.102 |
| DHCP Enabled | Yes |
| DHCP Server | 192.168.0.10 |
| DNS Server | 192.168.0.10 |

Verified that WIN11-01 appeared under:

```
DHCP
→ IPv4
→ Office LAN
→ Address Leases
```

Successfully verified network connectivity and domain communication after receiving a DHCP lease.

![DHCP Lease](images/05-dhcp-validation.png)

*Figure 15.5. Validating DHCP operation.*

# Verification
Verified that:

- The DHCP Server role was installed successfully.
- The DHCP Server was authorized in Active Directory.
- The Office LAN scope was created successfully.
- DHCP scope options were configured correctly.
- WIN11-01 successfully obtained an IP address automatically.
- The client received the correct Default Gateway.
- The client received DC01 as its DNS Server.
- WIN11-01 appeared in the DHCP Address Leases list.
- Domain communication remained functional after switching from a static IP to DHCP.

# Problems Encountered
## Client Initially Received an APIPA Address
After changing WIN11-01 from a static IP address to DHCP, the client initially received an Automatic Private IP Address (APIPA) in the **169.254.x.x** range instead of an address from the DHCP scope.

### Resolution
Verified the DHCP configuration, confirmed that the DHCP Server was authorized, checked the active scope and Address Leases, and renewed the client lease. The client successfully received a valid DHCP-assigned address (**192.168.0.102**) from DC01.

# Lessons Learned
- DHCP servers in an Active Directory environment must be authorized before issuing IP addresses.
- A DHCP scope defines the pool of IP addresses available to clients.
- DHCP options automatically provide clients with the default gateway, DNS server, and domain information.
- An APIPA address (169.254.x.x) indicates that a client could not initially obtain a DHCP lease.
- Address Leases provide an effective way to verify that DHCP is functioning correctly.

# Best Practices
- Reserve static IP addresses for infrastructure servers.
- Use DHCP for client computers whenever possible.
- Configure exclusions for devices that require static IP addresses.
- Verify DHCP functionality by checking both the client configuration and the Address Leases list.
- Use the Domain Controller as the DNS Server for all domain-joined clients.

# Summary
In this module, I installed and authorized the DHCP Server role on DC01, created an IPv4 scope, configured DHCP options, and activated the scope. I then converted WIN11-01 from a static IP configuration to DHCP and verified that it automatically received a valid IP address, default gateway, DNS server, and DHCP lease from DC01. The successful lease assignment confirmed that the DHCP infrastructure was operating correctly within the Active Directory environment.

# Next Module
**Module 16 - PowerShell Administration**