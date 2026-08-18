# Module 14 - Group Policy Management
> **Module Overview**
>
> This module covers the creation, configuration, and deployment of a Group Policy Object (GPO) to centrally manage user settings within an Active Directory domain. The module demonstrates how Group Policy can be linked to an Organizational Unit (OU) to apply user-specific restrictions while leaving other departments unaffected.

# Objective

Create and deploy a Group Policy Object that prevents Human Resources users from accessing Control Panel and Settings, then verify that the policy applies only to users in the HR Organizational Unit.

# Skills Demonstrated

- Group Policy administration
- Group Policy Object (GPO) creation
- Organizational Unit (OU) management
- User Configuration policies
- Group Policy deployment
- Group Policy troubleshooting
- Policy validation
- Technical documentation

# Technologies Used

| Technology | Purpose |
|------------|---------|
| Windows Server 2025 | Domain Controller |
| Group Policy Management | Create and manage GPOs |
| Active Directory Users and Computers | Organizational Unit management |
| Windows 11 Pro | Domain client |
| Command Prompt | Group Policy update |

# Lab Environment
| Component | Value |
|-----------|-------|
| Domain Controller | DC01 |
| Client Computer | WIN11-01 |
| Domain | kennethlab.test |
| Organizational Unit | HR |
| Group Policy Object | HR Restrictions |

# Prerequisites
Before beginning this module, I completed:

- Module 11 – Configure Windows 11 Client and Join the Active Directory Domain
- Module 12 – Active Directory Users and Group Management
- Module 13 – Shared Folder and NTFS Permissions

---

# Procedure
## Step 1 - Open Group Policy Management
### Purpose
Access the Group Policy Management Console to create and manage Group Policy Objects.

### Actions Performed
Opened:

```
Server Manager
→ Tools
→ Group Policy Management
```

Expanded the Active Directory domain:

```
Forest
└── Domains
    └── kennethlab.test
```

Verified the existing Group Policy Objects before creating a new policy.

![Group Policy Management Console](images/01-gpmc.png)

*Figure 14.1. Opening the Group Policy Management Console.*

## Step 2 - Create and Link the Group Policy Object
### Purpose
Create a Group Policy Object and link it to the HR Organizational Unit.

### Actions Performed
Right-clicked:

```
HR Organizational Unit
```

Selected:

```
Create a GPO in this domain, and Link it here...
```

Created the Group Policy Object:

```
HR Restrictions
```

The Group Policy Object was linked directly to the HR Organizational Unit.

![Create GPO](images/02-create-gpo.png)

*Figure 14.2. Creating and linking the HR Restrictions Group Policy Object.*

## Step 3 - Configure the Group Policy
### Purpose
Prevent HR users from accessing Control Panel and Settings.

### Actions Performed
Edited the **HR Restrictions** Group Policy Object.

Navigated to:

```
User Configuration
→ Policies
→ Administrative Templates
→ Control Panel
```

Configured:

```
Prohibit access to Control Panel and PC settings
```

Set the policy to:

```
Enabled
```

Saved the configuration.

![Configure Policy](images/03-configure-policy.png)

*Figure 14.3. Enabling the Control Panel restriction policy.*

## Step 4 - Update Group Policy
### Purpose
Force the client computer to retrieve the latest Group Policy settings.

### Actions Performed
Signed in to **WIN11-01** using the HR user account.

Executed:

```cmd
gpupdate /force
```

Confirmed that both User Policy and Computer Policy updated successfully.

Logged off and signed back in to apply the User Configuration policy.

![GPUpdate](images/04-gpupdate.png)

*Figure 14.4. Updating Group Policy on the client computer.*

## Step 5 - Validate Group Policy
### Purpose
Verify that the Group Policy applies only to the intended users.

### Positive Test
Signed in as:

```
KENNETHLAB\jane.smith
```

Attempted to open:

- Control Panel
- Windows Settings

Result:

Access was successfully blocked.

### Negative Test
Signed in as:

```
KENNETHLAB\john.reyes
```

Attempted to open:

- Control Panel

Result:

Control Panel opened successfully because the user belongs to the IT Organizational Unit.

![Policy Validation](images/05-policy-validation.png)

*Figure 14.5. Verifying Group Policy behavior using different user accounts.*

# Verification
Verified that:

- The HR Restrictions Group Policy Object was created successfully.
- The Group Policy Object was linked to the HR Organizational Unit.
- The Control Panel restriction policy was enabled.
- Group Policy updated successfully on WIN11-01.
- Jane Smith was unable to access Control Panel and Settings.
- John Reyes was able to access Control Panel normally.
- The Group Policy applied only to users within the HR Organizational Unit.

# Problems Encountered
## Group Policy Object Created in the Wrong Location

Initially, the Group Policy Object was created under the domain instead of being linked to the HR Organizational Unit. This did not match the intended scope of the policy.

### Resolution
Deleted the incorrectly placed Group Policy Object and recreated it by right-clicking the **HR Organizational Unit** and selecting **Create a GPO in this domain, and Link it here**. After applying the policy, only users in the HR Organizational Unit were affected, while users in the IT Organizational Unit remained unaffected.

# Validation Results
| Test User | Organizational Unit | Expected Result | Actual Result | Status |
|-----------|----------------------|-----------------|---------------|--------|
| Jane Smith | HR | Control Panel blocked | Unable to access Control Panel and Settings | ✅ Passed |
| John Reyes | IT | Control Panel available | Successfully opened Control Panel | ✅ Passed |

# Lessons Learned
- Group Policy Objects should be linked to the appropriate Organizational Unit to control which users or computers receive the policy.
- User Configuration policies typically require the user to sign out and sign back in after updating Group Policy.
- The `gpupdate /force` command immediately refreshes Group Policy settings on a client computer.
- Validating both successful and unsuccessful policy application confirms that the policy scope is configured correctly.

# Best Practices
- Link Group Policy Objects only to the Organizational Units that require the policy.
- Test Group Policy using multiple user accounts.
- Validate policy behavior after every configuration change.
- Use descriptive names for Group Policy Objects.

# Summary
In this module, I created and configured the **HR Restrictions** Group Policy Object to prevent HR users from accessing Control Panel and Windows Settings. After linking the policy to the HR Organizational Unit and updating Group Policy on the client computer, I verified that **Jane Smith** received the restriction while **John Reyes** remained unaffected. This demonstrated successful implementation of Organizational Unit-based policy targeting and centralized user management through Group Policy.

# Next Module
**Module 15 - Dynamic Host Configuration Protocol (DHCP)**