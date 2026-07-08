# Ransomware Incident Response Playbook

**Version:** 1.0 | **Author:** Harshit | **Task:** Week 4 – Task 1

---

## ACTIVATION TRIGGER
Activate this playbook when: files are encrypted, ransom note found, `.locked`/`.enc` extensions appear, OR SIEM alerts on mass file modification.

---

## ⏱️ PHASE 0 — First 15 Minutes

```
□ Photograph/screenshot ALL ransom notes before touching anything
□ Call Incident Commander immediately — declare P1
□ Open incident ticket → INC-YYYY-NNNN
□ Establish Signal group for IRT (assume email compromised)
□ DO NOT reboot any infected system
□ DO NOT try to decrypt files yourself
□ DO NOT pay ransom without legal and executive approval
```

---

## ⏱️ PHASE 1 — Detection & Scoping (0–2 hours)

### Identify Affected Systems
```bash
# Windows — find encrypted files
Get-ChildItem -Recurse C:\ -Include "*.locked","*.enc","*.crypted" 2>$null

# Linux — find ransom notes
find / -name "HOW_TO_DECRYPT*" -o -name "README_DECRYPT*" 2>/dev/null

# Check active network connections for C2
netstat -an | grep ESTABLISHED

# List recently modified files (last 24 hours)
find / -mtime -1 -type f 2>/dev/null | head -50
```

### Identify Ransomware Variant
1. Note the encrypted file extension
2. Upload sample + ransom note to: https://id-ransomware.malwarehunterteam.com/
3. Check for free decryptor: https://www.nomoreransom.org/
4. Record variant name for threat intelligence

### Scope Assessment Checklist
```
□ How many systems show encryption?
□ Are servers affected or only workstations?
□ Is the backup server reachable and clean?
□ Has data been exfiltrated? (check firewall for large outbound transfers)
□ Is the attack still spreading? (monitor SIEM for new encryption events)
□ What was the initial attack vector?
```

---

## ⏱️ PHASE 2 — Containment (2–6 hours)

### Network Isolation
```
Priority 1 — Stop spread:
□ VLAN-isolate ALL confirmed infected systems
□ Block attacker's external IPs at firewall
□ Disable compromised user accounts in Active Directory
□ Revoke all active VPN sessions

Priority 2 — Protect clean systems:
□ Verify backup servers are isolated (not reachable from infected network)
□ Take critical unaffected servers offline as precaution
□ Snapshot all AWS EC2 instances immediately
□ Enable read-only mode on S3 buckets containing backups
```

### Evidence Preservation (CRITICAL — Do Before Anything Else)
```
□ RAM dump: WinPmem.exe /output ram.raw  (preserves encryption keys!)
□ Capture network state: netstat -an > netstat.txt
□ Export Windows Event Logs before they rotate
□ Export SIEM data for the past 30 days
□ Forensic disk image: FTK Imager → create .E01 image
□ Document chain of custody for all evidence
```

### Ransom Payment Decision
```
Criteria to NEVER pay:
✅ Verified clean backups exist → RESTORE, do not pay
✅ Ransomware group is OFAC-sanctioned → ILLEGAL to pay
✅ Free decryptor exists on nomoreransom.org → USE IT

Criteria where payment MAY be considered (legal approval required):
⚠️  No backups exist
⚠️  Data loss would cause irreversible business damage
⚠️  Group is NOT sanctioned
⚠️  FBI and legal counsel consulted and approved

ALWAYS: Consult legal counsel and FBI before paying
```

---

## ⏱️ PHASE 3 — Eradication (6–48 hours)

```
□ Identify ALL persistence mechanisms:
  - Windows: schtasks /query | Get-ScheduledTask
  - Windows Registry: HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
  - Linux: crontab -l | /etc/cron* | systemctl list-units
  - Startup folders, WMI subscriptions, service installs

□ Remove all malware files (hash verify against known-bad hashes)
□ Delete all unauthorized accounts created by attacker
□ Remove all attacker SSH authorized_keys entries
□ Reimage ALL infected systems (preferred over manual cleanup)
□ Patch the initial access vulnerability BEFORE reconnecting
□ Reset ALL passwords (entire organization, not just affected users)
□ Rotate ALL API keys, certificates, and service account credentials
□ Verify clean with full EDR scan — zero detections before returning to network
```

---

## ⏱️ PHASE 4 — Recovery (48–168 hours)

### Restoration Priority
```
Tier 1 (restore first — revenue critical):
□ Payment processing systems
□ Customer database
□ Order management

Tier 2 (restore next):
□ Web application (customer-facing)
□ Inventory management
□ Email systems

Tier 3 (restore last):
□ Employee workstations (reimage from baseline)
□ Development environments
□ Non-critical tools
```

### Recovery Checklist
```
□ Verify backup integrity BEFORE restoring (check hashes)
□ Verify backup server itself was not compromised
□ Restore to clean, isolated environment first — test before production
□ Apply ALL security patches before reconnecting to network
□ Monitor intensively for 72 hours after recovery
□ Validate business operations end-to-end
□ Confirm customer data integrity
```

---

## ⏱️ PHASE 5 — Post-Incident (Within 2 weeks)

```
□ Schedule and conduct Post-Incident Review (PIR)
□ Document complete attack timeline
□ Identify and remediate root cause
□ Update IR plan with lessons learned
□ Add new IOCs to threat intelligence feeds
□ Add new detection rules to SIEM
□ Brief all security staff on incident and improvements
□ Conduct phishing awareness training for all employees
□ Commission follow-up penetration test
```

---

## Notification Requirements

| Stakeholder | When | Who Notifies |
|-------------|------|-------------|
| Executive Team | Within 30 min (P1) | CISO |
| CERT-In | Within 6 hours | CISO |
| Cyber Insurance | Within 24 hours | Legal |
| Customers (if PII affected) | Per CERT-In directive | Legal + Comms |
| Payment Card Brands | If card data affected | Legal |
| FBI/Law Enforcement | If criminal investigation needed | Legal + CEO |

---

*Playbook: Week 4 Task 1 | Reference: CISA Ransomware Guide, NIST SP 800-61*
