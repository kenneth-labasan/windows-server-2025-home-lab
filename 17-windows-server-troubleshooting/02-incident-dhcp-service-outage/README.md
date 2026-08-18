# Incident 2 - DHCP Service Outage
> **Incident Overview**
>
> This incident simulated a Dynamic Host Configuration Protocol (DHCP) service outage by stopping the DHCP Server service on the Domain Controller. The objective was to observe the client behavior, investigate the symptoms, identify the root cause, restore the DHCP service, and verify that normal network communication was restored.

# Objective
Demonstrate a structured troubleshooting process for diagnosing and resolving a DHCP service outage affecting a domain client.

# Skills Demonstrated
- DHCP troubleshooting
- Windows Services management
- Windows networking diagnostics
- Root cause analysis
- Incident documentation
- Service recovery
- Network validation

# Environment
| Component | Value |
|-----------|-------|
| Server | DC01 |
| Client | WIN11-01 |
| Domain | kennethlab.test |
| DHCP Server | DC01 (192.168.0.10) |

# Symptoms
The DHCP Server service was intentionally stopped to simulate a DHCP outage.

The following behavior was observed:

- The client was unable to obtain a new DHCP lease while the service was unavailable.
- The client did **not** receive an APIPA (169.254.x.x) address because an existing DHCP lease was still valid.
- `ping dc01` continued to succeed.
- `nslookup dc01` returned:

```
Server: Unknown
*** Can't find dc01
```

# Evidence Collected
Executed the following commands:

```cmd
ipconfig /all

ipconfig /release

ipconfig /renew

ping dc01

nslookup dc01
```

### Observations
| Test | Result |
|------|--------|
| DHCP Server Service | Stopped |
| ipconfig /renew | Unable to obtain a new DHCP lease while the service was unavailable |
| ping dc01 | Successful |
| nslookup dc01 | Server: Unknown / Can't find dc01 |

# Investigation
The client continued communicating with the Domain Controller because it still held a valid DHCP lease.

Although basic network connectivity remained available, DNS-related operations did not function as expected while the DHCP service was unavailable.

The investigation focused on verifying the status of the DHCP Server service rather than assuming a client-side networking issue.

# Root Cause
The DHCP Server service on **DC01** was intentionally stopped, preventing clients from renewing or obtaining DHCP leases.

Because the existing lease had not yet expired, the client retained network connectivity instead of immediately assigning itself an APIPA address.

# Resolution
Started the **DHCP Server** service on DC01.

Executed on WIN11-01:

```cmd
ipconfig /renew
```

Verified that the client successfully renewed its network configuration.

# Validation
Executed:

```cmd
ipconfig /all

ping dc01

nslookup dc01
```

Verified:

- DHCP Server service returned to **Running**.
- DNS Server restored to **192.168.0.10**.
- Successful DHCP lease renewal.
- Successful DNS name resolution.
- Successful communication with the Domain Controller.

# Lessons Learned
- A stopped DHCP Server service does not always cause clients to receive an APIPA address immediately.
- Clients with a valid DHCP lease may continue operating until the lease expires.
- Service availability should be verified before modifying client network settings.
- Network connectivity and DHCP lease renewal are related but separate processes.

# Prevention
- Monitor critical Windows services such as DHCP Server.
- Configure service monitoring and alerting in production environments.
- Verify DHCP service availability before troubleshooting client devices.
- Document DHCP scope configuration and service dependencies.

# Engineering Notes
This incident demonstrates that Windows DHCP clients do not always behave exactly as textbook examples suggest. Existing DHCP leases can allow continued network communication even when the DHCP service becomes unavailable.

This reinforces the importance of collecting evidence before forming conclusions during troubleshooting.

# Key Takeaways
- Successfully simulated a DHCP service outage.
- Verified the effect of stopping the DHCP Server service.
- Investigated client behavior during a DHCP outage.
- Restored the DHCP service.
- Renewed the client lease.
- Verified successful recovery of DHCP and DNS functionality.