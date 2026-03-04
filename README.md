# <a href="https://www.linkedin.com/in/RayJohnson/">Raymond Johnson</a>'s IT and Cybersecurity Project Portfolio 🔐 

What started as a simple home lab has turned into a full-on playground for learning how attackers think and how defenders respond. I use it to test ideas, break patterns, build detections, and understand threats from the inside out. These projects aren’t just exercises—they’re snapshots of how I’m leveling up as a future SOC analyst. If you want to see the kind of work that fuels my curiosity and keeps me pushing forward, my lab is the best place to start.

## ⚠️ Vulnerability Management Projects

- **[Vulnerability Management Program Implementation](https://github.com/RayjCy83r78/vulnerability-management-program/tree/main)**
- **[Programmatic Vulnerability Remediations (PowerShell and BASH)](https://github.com/joshcybertest/programmatic-vulnerability-remediations)**

## 🚨 


## 🤳 Connect With Me


[<img align="left" alt="Ray Johnson | LinkedIn" width="22px" src="https://cdn.jsdelivr.net/npm/simple-icons@v3/icons/linkedin.svg" />][linkedin]

[linkedin]: https://www.linkedin.com/in/ray-johnson-3606061a0



# Windows 11 DISA STIG Compliance Lab — Tenable Vulnerability Management

## Overview

This project documents a hands-on DISA STIG compliance audit performed using **Tenable Vulnerability Management** against a Windows 11 virtual machine. The VM was intentionally misconfigured to simulate a poorly hardened system. A full compliance scan was executed using the official **DISA STIG: Microsoft Windows 11 V2R6** audit policy. Findings were documented, remediated, and verified through re-scanning.

---

## Environment

| Component | Details |
|-----------|---------|
| Scanner | Tenable Vulnerability Management (Tenable.io) |
| Audit Policy | DISA STIG — Microsoft Windows 11 V2R6 |
| Target | Windows 11 VM (isolated lab environment) |
| Scan Type | Credentialed Compliance + Vulnerability Scan |
| Scan Date | March 3, 2026 |

---

## Methodology

1. Configured a **credentialed compliance scan** in Tenable.io using the DISA Windows 11 STIG V2R6 audit policy
2. Intentionally misconfigured the Windows 11 VM across multiple security domains to simulate a real-world poorly hardened endpoint
3. Ran an initial scan to establish a **baseline of failed controls**
4. Documented all failed STIG controls with VULN-ID, STIG-ID, severity, and remediation steps
5. Remediated controls using `secpol.msc`, `gpedit.msc`, `wf.msc`, Registry Editor, and PowerShell
6. Re-ran the scan to **verify remediation** and confirm controls passed

---

## Vulnerability Summary (Pre-Remediation)

| Severity | Count |
|----------|-------|
| Critical | 0 |
| High | 3 |
| Medium | 4 |
| Low | 2 |
| Info | 125 |
| **Total** | **134** |

> High severity findings include unpatched Microsoft Outlook, Microsoft Teams RCE, and WinVerifyTrust signature validation.

---

## STIG Audit Failures — Selected Findings

The following 15 STIG controls were targeted for remediation. All represent real-world, high-impact misconfigurations commonly found on default Windows 11 installations.

| # | STIG-ID | VULN-ID | Title | Severity | Status |
|---|---------|---------|-------|----------|--------|
| 1 | WN11-AC-000005 | V-253274 | Account lockout duration must be 15 minutes or greater | Medium | FAILED → PASSED |
| 2 | WN11-AC-000010 | V-253275 | Bad logon attempts must be configured to 3 or less | Medium | FAILED → PASSED |
| 3 | WN11-AC-000015 | V-253276 | Lockout counter reset must be configured to 15 minutes | Medium | FAILED → PASSED |
| 4 | WN11-AC-000020 | V-253277 | Password history must be configured to 24 passwords | Medium | FAILED → PASSED |
| 5 | WN11-AC-000025 | V-253278 | Maximum password age must be 60 days or less | Medium | FAILED → PASSED |
| 6 | WN11-AC-000035 | V-253280 | Passwords must be at minimum 14 characters | Medium | FAILED → PASSED |
| 7 | WN11-AC-000040 | V-253281 | Built-in Microsoft password complexity filter must be enabled | Medium | FAILED → PASSED |
| 8 | WN11-SO-000075 | V-253344 | Required legal notice must display before console logon | Medium | FAILED → PASSED |
| 9 | WN11-SO-000080 | V-253345 | Windows message title for legal notice must be configured | Low | FAILED → PASSED |
| 10 | WN11-CC-000038 | V-253302 | WDigest Authentication must be disabled | High | FAILED → PASSED |
| 11 | WN11-CC-000190 | V-253318 | Autoplay must be disabled for all drives | Medium | FAILED → PASSED |
| 12 | WN11-SO-000205 | V-253365 | LanMan authentication level must be set to NTLMv2 only | High | FAILED → PASSED |
| 13 | WN11-00-000135 | V-253264 | Host-based firewall must be installed and enabled | High | FAILED → PASSED |
| 14 | WN11-SO-000150 | V-253355 | Anonymous enumeration of shares must be restricted | Medium | FAILED → PASSED |
| 15 | WN11-CC-000326 | V-253331 | PowerShell script block logging must be enabled | Medium | FAILED → PASSED |

---

## Remediation Steps

### 1–7 — Password & Account Lockout Policy
**Tool:** `secpol.msc` → Account Policies → Password Policy / Account Lockout Policy

```
Account Lockout Duration       → 15 minutes
Account Lockout Threshold      → 3 invalid attempts
Reset Lockout Counter After    → 15 minutes
Password History               → 24 passwords remembered
Maximum Password Age           → 60 days
Minimum Password Length        → 14 characters
Password Complexity            → Enabled
```

---

### 8–9 — DoD Legal Notice Banner
**Tool:** `secpol.msc` → Local Policies → Security Options

```
Interactive logon: Message title for users → DoD Notice and Consent Banner
Interactive logon: Message text for users  → [Full DoD warning banner text]
```

---

### 10 — WDigest Authentication
**Tool:** Registry Editor

```
Path:  HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest
Value: UseLogonCredential
Type:  DWORD
Data:  0
```

---

### 11 — Autoplay Disabled
**Tool:** `gpedit.msc` → Computer Configuration → Administrative Templates → Windows Components → AutoPlay Policies

```
Turn off AutoPlay → Enabled → All Drives
```

---

### 12 — LanMan Authentication Level
**Tool:** `secpol.msc` → Local Policies → Security Options

```
Network security: LAN Manager authentication level
→ Send NTLMv2 response only. Refuse LM & NTLM
```

---

### 13 — Host-Based Firewall
**Tool:** `wf.msc` → Windows Defender Firewall Properties

```
Domain Profile   → Firewall state: On | Inbound: Block
Private Profile  → Firewall state: On | Inbound: Block
Public Profile   → Firewall state: On | Inbound: Block
```

**PowerShell (alternative):**
```powershell
Set-NetFirewallProfile -Profile Domain,Private,Public -Enabled True
Set-NetFirewallProfile -Profile Domain,Private,Public -DefaultInboundAction Block
```

---

### 14 — Anonymous Share Enumeration
**Tool:** `secpol.msc` → Local Policies → Security Options

```
Network access: Do not allow anonymous enumeration of SAM accounts and shares → Enabled
```

---

### 15 — PowerShell Script Block Logging
**Tool:** `gpedit.msc` → Computer Configuration → Administrative Templates → Windows Components → Windows PowerShell

```
Turn on PowerShell Script Block Logging → Enabled
```

---

## Scan Reports

| Report | Description |
|--------|-------------|
| [Before Remediation](scans/before_remediation.pdf) | Initial Tenable scan showing all STIG failures |
| [After Remediation](scans/after_remediation.pdf) | Post-remediation scan confirming controls passed |

---

## Key Takeaways

- Default Windows 11 installations fail a significant number of DISA STIG controls out of the box
- High-severity findings like **WDigest authentication** and **LanMan authentication level** are trivial to remediate but carry serious security impact if left unaddressed — WDigest in particular exposes plaintext credentials in memory
- **Credentialed scanning** in Tenable.io is essential for accurate compliance results; unauthenticated scans cannot check registry values, audit policies, or local security settings
- Remediating **firewall misconfigurations** across all three profiles (Domain, Private, Public) is one of the highest-impact quick wins on any Windows endpoint
- STIG compliance is not a one-time task — periodic re-scanning is necessary to detect configuration drift

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Tenable Vulnerability Management | Compliance scanning and vulnerability detection |
| `secpol.msc` | Password policy, account lockout, security options |
| `gpedit.msc` | Group policy — AutoPlay, PowerShell logging |
| `wf.msc` | Windows Defender Firewall configuration |
| Registry Editor | WDigest, LanMan, and other registry-based controls |
| PowerShell | Firewall scripting, SMB configuration |

---

## References

- [DISA STIG Library — public.cyber.mil](https://public.cyber.mil/stigs/)
- [DISA Windows 11 STIG V2R6](https://dl.dod.cyber.mil/wp-content/uploads/stigs/zip/U_MS_Windows_11_V2R6_STIG.zip)
- [Tenable.io Documentation](https://docs.tenable.com)
- [NIST SP 800-53 Rev 5](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final)
- [NIST SP 800-171](https://csrc.nist.gov/publications/detail/sp/800-171/rev-3/final)
