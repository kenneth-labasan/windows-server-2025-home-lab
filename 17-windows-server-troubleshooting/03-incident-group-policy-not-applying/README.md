# Incident 3 - Group Policy Not Applying
> **Incident Overview**
>
> This incident simulated a Group Policy issue by disabling the Group Policy Object (GPO) link for the Human Resources Organizational Unit. The objective was to investigate why the policy was no longer applied, identify the root cause, restore the configuration, and verify that the policy was successfully enforced again.

# Objective
Demonstrate a structured troubleshooting process for resolving a Group Policy application issue affecting a domain user.

# Skills Demonstrated
- Group Policy troubleshooting
- Active Directory administration
- Organizational Unit verification
- Group Policy diagnostics
- Root cause analysis
- Incident documentation
- Policy validation

# Environment
| Component | Value |
|-----------|-------|
| Server | DC01 |
| Client | WIN11-01 |
| Domain | kennethlab.test |
| User | KENNETHLAB\jane.smith |
| Policy | HR Restrictions |

# Symptoms
A Human Resources user reported that Control Panel was accessible even though a Group Policy should prevent access.

The following symptoms were observed:

- Jane Smith could successfully open Control Panel.
- `gpresult /r` did not list **HR Restrictions** under Applied Group Policy Objects.
- The GPO still existed within Group Policy Management.

# Evidence Collected
Executed the following command:

```cmd
gpresult /r
```

Verified:

- HR Restrictions was not listed under **Applied Group Policy Objects**.

Additional verification:

- Jane Smith remained inside the **HR Organizational Unit**.
- HR Restrictions existed under **Group Policy Objects**.
- The GPO link for the HR Organizational Unit was disabled.

# Investigation
The investigation followed a structured process:

1. Verified the affected user account.
2. Confirmed the user remained in the correct Organizational Unit.
3. Confirmed the Group Policy Object still existed.
4. Reviewed the Group Policy link configuration.
5. Verified that the GPO link had been disabled.

This eliminated the possibility of user misplacement, accidental deletion of the GPO, or incorrect policy configuration.

# Root Cause
The **HR Restrictions** Group Policy Object still existed and contained the correct settings, but its **link to the HR Organizational Unit had been disabled**.

Because the link was disabled, the policy was no longer processed for users within the Human Resources Organizational Unit.

# Resolution
Performed the following actions:

- Re-enabled the **HR Restrictions** GPO link.
- Executed:

```cmd
gpupdate /force
```

- Logged back in as:

```
KENNETHLAB\jane.smith
```

- Executed:

```cmd
gpresult /r
```

# Validation
Verified that:

- HR Restrictions appeared under **Applied Group Policy Objects**.
- Control Panel access was blocked again.
- The Group Policy link was enabled.
- Group Policy processing completed successfully.

# Troubleshooting Checklist
- ✅ Verified affected user account
- ✅ Verified Organizational Unit placement
- ✅ Verified Group Policy Object existed
- ✅ Verified Group Policy link status
- ✅ Updated Group Policy
- ✅ Verified applied policies using `gpresult /r`
- ✅ Confirmed policy functionality

# Lessons Learned
- A Group Policy Object can exist and contain the correct settings while still failing to apply.
- Group Policy links should always be verified before modifying policy settings.
- `gpresult /r` is one of the most effective tools for verifying whether a policy is applied.
- Troubleshooting should begin by verifying configuration rather than immediately recreating policies.

# Prevention
- Verify GPO link status before editing or recreating Group Policy Objects.
- Document Organizational Unit structure and linked policies.
- Validate Group Policy changes using `gpupdate /force` and `gpresult /r`.
- Review linked policies after administrative changes.

# Engineering Notes
This incident demonstrates the relationship between Active Directory Organizational Units and Group Policy processing.

A correctly configured Group Policy Object will not affect users unless it is properly linked to the Organizational Unit containing those users. Verifying the existence of the policy alone is insufficient; administrators must also verify the link status and policy application.

# Key Takeaways
- Successfully simulated a Group Policy application failure.
- Used `gpresult /r` to verify policy application.
- Confirmed user placement within the correct Organizational Unit.
- Identified a disabled GPO link as the root cause.
- Restored policy application by re-enabling the link.
- Validated successful enforcement of the HR Restrictions policy.

# Real-World Administrator Tip
When a user reports that a Group Policy is not working, avoid immediately editing or recreating the policy.

Instead, verify the following in order:

1. Is the user in the correct Organizational Unit?
2. Does the Group Policy Object still exist?
3. Is the GPO linked to the Organizational Unit?
4. Is the link enabled?
5. Does `gpresult /r` show the policy as applied?

Following this process reduces unnecessary configuration changes and helps identify the root cause more efficiently.