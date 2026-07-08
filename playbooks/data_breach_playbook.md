# Data Breach Incident Response Playbook

**Version:** 1.0 | **Author:** Harshit | **Task:** Week 4 – Task 1

---

## ACTIVATION TRIGGER
Activate when: unauthorized access to PII/PCI data confirmed or suspected, large outbound transfer detected, external notification of breach received, or customer reports identity theft linked to ShopNow.

---

## ⏱️ PHASE 0 — First 15 Minutes

```
□ Declare P1 incident — assemble IRT immediately
□ Engage Legal Counsel — breach has regulatory implications
□ DO NOT delete any logs or data (legal hold)
□ Establish secure out-of-band communication
□ Block ongoing exfiltration at firewall if still active
□ Open incident ticket and begin documentation
```

---

## ⏱️ PHASE 1 — Detection & Scoping (0–2 hours)

### Confirm the Breach
```
□ Identify WHAT data was accessed (PII, PCI, credentials, IP)
□ Identify HOW MUCH data (number of records, file sizes)
□ Identify WHO accessed it (external attacker, insider, third party)
□ Identify WHEN the breach occurred (start and end time)
□ Identify WHERE data was sent (destination IPs, domains)
□ Determine if breach is still ONGOING or already completed
```

### AWS Data Exfiltration Investigation
```bash
# Check CloudTrail for S3 GetObject events (data reads)
aws cloudtrail lookup-events \
  --lookup-attributes AttributeKey=EventName,AttributeValue=GetObject \
  --start-time 2025-06-01T00:00:00Z

# Check for large S3 downloads
aws s3api list-objects --bucket shopnow-customer-data \
  --query 'Contents[?Size>`1000000`]'

# Review VPC Flow Logs for large outbound transfers
# Filter: direction=egress, bytes > 10MB
```

### Database Breach Investigation
```sql
-- Check database audit logs for bulk SELECT
SELECT user, host, query, rows_examined, timestamp
FROM mysql.general_log
WHERE argument LIKE 'SELECT%' 
  AND rows_examined > 10000
  AND timestamp > NOW() - INTERVAL 7 DAY
ORDER BY rows_examined DESC;

-- Check for new database users
SELECT user, host, password_last_changed
FROM mysql.user
WHERE password_last_changed > NOW() - INTERVAL 30 DAY;
```

---

## ⏱️ PHASE 2 — Containment (2–4 hours)

```
□ Block attacker's IPs/accounts from further access
□ Revoke compromised credentials and API keys
□ Disable exfiltration pathway (block destination IPs)
□ Enable S3 Block Public Access if misconfiguration was the cause
□ Force re-authentication for ALL users (invalidate sessions)
□ Enable enhanced CloudTrail logging if not already active
□ Preserve evidence: export all logs before rotating
```

---

## ⏱️ PHASE 3 — Regulatory Notification (Critical Path)

```
CERT-In (within 6 hours):
□ Submit notification at https://www.cert-in.org.in/
□ Include: nature of incident, systems affected, data involved,
  steps taken, contact person

If EU customers affected — GDPR (within 72 hours):
□ Notify relevant Data Protection Authority
□ Assess if customers must be individually notified
□ Document the decision and rationale

If payment card data involved — PCI-DSS (immediately):
□ Notify acquiring bank
□ Notify Visa: usfraud@visa.com
□ Notify Mastercard: compromised@mastercard.com
□ Engage PCI Forensic Investigator (PFI)

Customer Notification (timing per legal guidance):
□ Draft notification with legal review
□ Include: what happened, what data, what to do, contact
□ Send via email + post prominent notice on website
□ Set up dedicated breach hotline/FAQ page
```

---

## ⏱️ PHASE 4 — Eradication & Recovery

```
□ Remove attacker access (accounts, SSH keys, API keys)
□ Patch exploited vulnerability
□ Enable encryption for data at rest (if not already)
□ Implement data minimization (delete data no longer needed)
□ Review and restrict database access permissions
□ Implement DLP solution to detect future exfiltration
□ Add monitoring for bulk data access patterns
```

---

## Breach Notification Template

```
Subject: Important Security Notice Regarding Your ShopNow Account

Dear [Customer Name],

We are writing to inform you of a security incident at ShopNow
that may have involved your personal information.

WHAT HAPPENED:
Between [DATE] and [DATE], an unauthorized party gained access
to ShopNow's systems and may have accessed personal information.

WHAT INFORMATION MAY BE INVOLVED:
• Full name and email address
• Shipping address
• [Order history / Payment method (last 4 digits only)]

WHAT WE ARE DOING:
• We have secured our systems and removed unauthorized access
• We have engaged leading cybersecurity experts
• We have notified relevant authorities including CERT-In
• We are implementing additional security measures

WHAT YOU SHOULD DO:
1. Monitor your accounts for unusual activity
2. Change your ShopNow password: [LINK]
3. Enable two-factor authentication
4. Be alert for phishing emails claiming to be from ShopNow

For questions: breach-support@shopnow.com | 1800-XXX-XXXX

We sincerely apologize for this incident.
[CISO Name] | Chief Information Security Officer, ShopNow
```

---

*Playbook: Week 4 Task 1 | Reference: CERT-In Directions 2022, GDPR Article 33, PCI-DSS v4.0*
