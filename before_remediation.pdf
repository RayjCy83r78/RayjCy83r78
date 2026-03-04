# STIG Findings Log — Windows 11 DISA STIG V2R6

**Scan Tool:** Tenable Vulnerability Management  
**Audit Policy:** DISA STIG Microsoft Windows 11 V2R6  
**Scan Date:** March 3, 2026  
**Target:** Windows 11 VM (isolated lab)

---

## Findings Table

| # | STIG-ID | VULN-ID | Title | CAT | Before | After | Remediation Tool | Fix Applied |
|---|---------|---------|-------|-----|--------|-------|-----------------|-------------|
| 1 | WN11-AC-000005 | V-253274 | Account lockout duration must be 15 minutes or greater | II | FAILED | PASSED | secpol.msc | Account Lockout Policy → Duration = 15 min |
| 2 | WN11-AC-000010 | V-253275 | Bad logon attempts must be configured to 3 or less | II | FAILED | PASSED | secpol.msc | Account Lockout Policy → Threshold = 3 |
| 3 | WN11-AC-000015 | V-253276 | Lockout counter reset must be 15 minutes | II | FAILED | PASSED | secpol.msc | Account Lockout Policy → Reset counter = 15 min |
| 4 | WN11-AC-000020 | V-253277 | Password history must be 24 passwords remembered | II | FAILED | PASSED | secpol.msc | Password Policy → History = 24 |
| 5 | WN11-AC-000025 | V-253278 | Maximum password age must be 60 days or less | II | FAILED | PASSED | secpol.msc | Password Policy → Max age = 60 days |
| 6 | WN11-AC-000035 | V-253280 | Passwords must be minimum 14 characters | II | FAILED | PASSED | secpol.msc | Password Policy → Min length = 14 |
| 7 | WN11-AC-000040 | V-253281 | Password complexity filter must be enabled | II | FAILED | PASSED | secpol.msc | Password Policy → Complexity = Enabled |
| 8 | WN11-SO-000075 | V-253344 | Legal notice must display before console logon | II | FAILED | PASSED | secpol.msc | Security Options → Interactive logon message text |
| 9 | WN11-SO-000080 | V-253345 | Legal notice title must be configured | III | FAILED | PASSED | secpol.msc | Security Options → Interactive logon message title |
| 10 | WN11-CC-000038 | V-253302 | WDigest Authentication must be disabled | II | FAILED | PASSED | Registry | HKLM\...\WDigest → UseLogonCredential = 0 |
| 11 | WN11-CC-000190 | V-253318 | Autoplay must be disabled for all drives | II | FAILED | PASSED | gpedit.msc | AutoPlay Policies → Turn off AutoPlay = All Drives |
| 12 | WN11-SO-000205 | V-253365 | LanMan auth level must be NTLMv2 only | I | FAILED | PASSED | secpol.msc | Security Options → LM auth level = NTLMv2 only |
| 13 | WN11-00-000135 | V-253264 | Host-based firewall must be enabled | II | FAILED | PASSED | wf.msc / PowerShell | All profiles → Firewall On, Inbound Block |
| 14 | WN11-SO-000150 | V-253355 | Anonymous enumeration of shares must be restricted | II | FAILED | PASSED | secpol.msc | Security Options → No anonymous enumeration of SAM/shares |
| 15 | WN11-CC-000326 | V-253331 | PowerShell script block logging must be enabled | II | FAILED | PASSED | gpedit.msc | Windows PowerShell → Script Block Logging = Enabled |

---

## CAT Level Reference

| Level | Meaning |
|-------|---------|
| CAT I | High severity — directly enables access or escalation |
| CAT II | Medium severity — degrades security or provides information useful to attackers |
| CAT III | Low severity — degrades defense-in-depth measures |

---

## Notable Finding Details

### WN11-CC-000038 — WDigest Authentication (CAT II)
WDigest is a legacy authentication protocol that stores credentials in plaintext in memory (LSASS). When enabled, attackers who gain access to memory (e.g., via Mimikatz) can harvest cleartext passwords. This is one of the most commonly exploited misconfigurations in penetration tests.

**Registry fix:**
```
HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest
UseLogonCredential = 0 (DWORD)
```

---

### WN11-SO-000205 — LanMan Authentication Level (CAT I)
Allowing LM and NTLM authentication enables legacy credential attacks including pass-the-hash and NTLM relay attacks. Setting to NTLMv2 only significantly reduces the attack surface.

**Fix via secpol.msc:**
```
Network security: LAN Manager authentication level
→ Send NTLMv2 response only. Refuse LM & NTLM
```

---

### WN11-00-000135 — Host-Based Firewall (CAT II)
Disabling Windows Defender Firewall across all profiles exposes the system to unrestricted inbound connections. This was remediated by enabling the firewall and setting default inbound rules to Block on all three profiles.

**PowerShell fix:**
```powershell
Set-NetFirewallProfile -Profile Domain,Private,Public -Enabled True
Set-NetFirewallProfile -Profile Domain,Private,Public -DefaultInboundAction Block
```

---

*Report generated as part of a DISA STIG compliance lab project for portfolio purposes.*
