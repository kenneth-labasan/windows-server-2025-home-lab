# Incident 1 - DNS Name Resolution Failure

> **Incident Overview**
>
> This incident simulated a DNS name resolution failure on a domain-joined Windows 11 client by intentionally changing the client to use an external DNS server instead of the internal Active Directory DNS server. The objective was to investigate the symptoms, identify the root cause, restore the correct configuration, and verify that Active Directory name resolution was functioning correctly.

# Objective
Demonstrate a structured troubleshooting process for resolving DNS name resolution issues affecting a domain-joined Windows client.

# Skills Demonstrated
- DNS troubleshooting
- Windows networking diagnostics
- Active Directory troubleshooting
- Root cause analysis
- Incident documentation
- Network validation

# Environment
| Component | Value |
|-----------|-------|
| Server | DC01 |
| Client | WIN11-01 |
| Domain | kennethlab.test |
| DNS Server (Healthy) | 192.168.0.10 |

# Symptoms
After modifying the client's DNS configuration, the following symptoms were observed:

- DNS server manually changed to **8.8.8.8**
- `nslookup dc01` failed.
- `ping dc01` failed because the hostname could not be resolved.
- `ping 192.168.0.10` succeeded, confirming basic network connectivity.

# Evidence Collected
Executed the following commands:

```cmd
ipconfig /all

ping dc01

ping 192.168.0.10

nslookup dc01
```

### Observations
| Test | Result |
|------|--------|
| ipconfig /all | DNS Server changed to 8.8.8.8 |
| ping dc01 | Failed |
| ping 192.168.0.10 | Successful |
| nslookup dc01 | Failed |

# Investigation
The client retained network connectivity because communication by IP address was still possible.

However, the client could no longer resolve the hostname **DC01** because it was querying Google's public DNS server instead of the internal DNS server hosted by the Domain Controller.

Since the **kennethlab.test** zone exists only on DC01, the external DNS server could not resolve internal Active Directory records.

# Root Cause
The DNS Server on WIN11-01 was manually changed from the internal Active Directory DNS server (**192.168.0.10**) to Google's public DNS server (**8.8.8.8**).

Because the public DNS server does not host the **kennethlab.test** zone, internal name resolution failed.

# Resolution
Restored the DNS Server Assignment to:

```
Automatic (DHCP)
```

Renewed the network configuration.

Executed:

```cmd
ipconfig /flushdns

ipconfig /renew
```

# Validation
Executed:

```cmd
ipconfig /all

ping dc01

nslookup dc01
```

Verified:

- DNS Server restored to **192.168.0.10**
- Successful hostname resolution
- Successful communication with DC01
- Active Directory DNS functionality restored

# Lessons Learned
- Active Directory relies heavily on DNS.
- Successful IP connectivity does not guarantee successful DNS resolution.
- `nslookup` is more appropriate than `ping` when validating DNS.
- Domain clients should use the Domain Controller as their DNS server.

# Prevention
- Configure domain clients to obtain DNS settings from DHCP.
- Avoid manually assigning public DNS servers to domain-joined computers.
- Verify DNS configuration before troubleshooting Active Directory issues.

# Engineering Notes
This incident demonstrates one of the most common causes of Active Directory communication problems: an incorrect DNS configuration.

Although the network itself remained operational, the client could no longer locate domain resources because it was querying a DNS server that did not host the organization's internal DNS zone.

# Key Takeaways
- Successfully simulated a DNS failure.
- Used Windows networking tools to collect evidence.
- Identified the root cause through structured troubleshooting.
- Restored the correct DNS configuration.
- Verified successful recovery of Active Directory name resolution.