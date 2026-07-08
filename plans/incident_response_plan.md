# Incident Response Plan

**Organization:** ShopNow E-Commerce Platform (Hypothetical)
**Document Version:** 2.0
**Author:** Harshit
**Task:** Week 4 – Task 1 | Cybersecurity Internship
**Framework:** NIST SP 800-61 Rev 2
**Last Updated:** June 2025
**Classification:** Confidential

---

## 1. Purpose and Scope

### 1.1 Purpose

This Incident Response Plan (IRP) establishes the procedures, roles, responsibilities, and guidelines for detecting, responding to, and recovering from cybersecurity incidents at ShopNow. The plan ensures a coordinated, efficient response that minimizes damage, reduces recovery time, and prevents recurrence.

### 1.2 Scope

This plan applies to:
- All information systems owned or operated by ShopNow
- All employees, contractors, and third parties with access to ShopNow systems
- All incidents affecting the confidentiality, integrity, or availability of ShopNow data and systems
- Cloud infrastructure (AWS), on-premise servers, and employee endpoints

### 1.3 Objectives

1. Minimize the impact of security incidents on business operations
2. Protect customer and company data from unauthorized access
3. Ensure compliance with legal and regulatory notification requirements
4. Preserve evidence for forensic analysis and legal proceedings
5. Learn from incidents to continuously improve security posture

---

## 2. Incident Definition and Classification

### 2.1 What Constitutes an Incident?

A **security incident** is any event that actually or potentially:
- Compromises the confidentiality of sensitive data
- Threatens the integrity of systems or data
- Disrupts the availability of critical systems
- Violates acceptable use or security policies

**Examples of incidents:**
- Ransomware or malware infection
- Unauthorized access to systems or data
- Data breach or exfiltration
- Phishing attack resulting in credential compromise
- Insider threat or policy violation
- DDoS attack causing service disruption
- Physical security breach of server room

**Not incidents (events only):**
- Failed login attempts below threshold
- Automated vulnerability scanner hitting the WAF
- Spam email that was automatically filtered

### 2.2 Severity Classification

```
┌────────────┬───────┬──────────────────────────────────────┬──────────────┐
│  Severity  │  P    │  Criteria                            │  Response    │
├────────────┼───────┼──────────────────────────────────────┼──────────────┤
│  CRITICAL  │  P1   │  Active ransomware, confirmed data    │  15 minutes  │
│            │       │  breach, core systems down, AWS root  │              │
│            │       │  compromise                           │              │
├────────────┼───────┼──────────────────────────────────────┼──────────────┤
│  HIGH      │  P2   │  Malware detected, unauthorized       │  1 hour      │
│            │       │  admin access, large data transfer,   │              │
│            │       │  VPN compromise                       │              │
├────────────┼───────┼──────────────────────────────────────┼──────────────┤
│  MEDIUM    │  P3   │  Policy violation, suspicious user    │  4 hours     │
│            │       │  behavior, phishing attempt,          │              │
│            │       │  failed privilege escalation          │              │
├────────────┼───────┼──────────────────────────────────────┼──────────────┤
│  LOW       │  P4   │  Minor anomaly, isolated failed       │  24 hours    │
│            │       │  logins, informational finding        │              │
└────────────┴───────┴──────────────────────────────────────┴──────────────┘
```

---

## 3. Incident Response Team

### 3.1 Team Structure

```
                    ┌─────────────────────┐
                    │  INCIDENT COMMANDER  │
                    │  (CISO / Security   │
                    │   Manager)          │
                    └──────────┬──────────┘
                               │
          ┌────────────────────┼────────────────────┐
          │                    │                    │
┌─────────▼────────┐ ┌────────▼─────────┐ ┌───────▼──────────┐
│  TECHNICAL LEAD  │ │  COMMUNICATIONS  │ │  LEGAL / COMPLIANCE│
│  Security Analyst│ │  LEAD            │ │  COUNSEL          │
│  Forensics       │ │  PR / Marketing  │ │  DPO / Privacy    │
│  IT Operations   │ │  Executive brief │ │  Regulatory       │
└──────────────────┘ └──────────────────┘ └──────────────────┘
```

### 3.2 Roles and Responsibilities

| Role | Responsibilities | Contact |
|------|-----------------|---------|
| **Incident Commander** | Overall coordination, decisions, escalation | CISO |
| **Technical Lead** | Investigation, containment, eradication | Security Lead |
| **Forensics Analyst** | Evidence collection, timeline reconstruction | Security Team |
| **IT Operations** | System isolation, restoration, patching | IT Manager |
| **Legal Counsel** | Regulatory compliance, legal guidance | General Counsel |
| **Communications Lead** | Internal/external messaging, PR | CMO |
| **Executive Sponsor** | Resource authorization, business decisions | CEO/CTO |

### 3.3 Contact Matrix

| Role | Primary Contact | Backup | Method |
|------|----------------|--------|--------|
| Incident Commander | security-lead@shopnow.com | ciso@shopnow.com | Phone + Signal |
| Technical Lead | analyst@shopnow.com | it-lead@shopnow.com | Phone |
| Legal | legal@shopnow.com | external-counsel | Phone |
| AWS Support | AWS Enterprise Support | — | Support Portal |
| FBI Cyber | ic3.gov | 1-800-CALL-FBI | Phone |
| CERT-In | incident@cert-in.org.in | — | Email + Phone |

---

## 4. Detection and Reporting

### 4.1 Detection Sources

| Source | Tool | Who Monitors |
|--------|------|-------------|
| SIEM Alerts | Wazuh / Elastic | SOC Analyst |
| EDR Detections | CrowdStrike | SOC Analyst |
| User Reports | Help Desk | IT Support |
| Firewall/IPS | Palo Alto | Network Team |
| AWS CloudTrail | AWS Security Hub | Cloud Team |
| Email Security | Proofpoint | Security Team |

### 4.2 Reporting Procedure

```
Detection Event
      │
      ▼
STEP 1: Log to incident tracking system (Jira / ServiceNow)
      │
      ▼
STEP 2: Assess severity (P1–P4) using classification matrix
      │
      ├─── P1/P2 ──▶ Notify Incident Commander IMMEDIATELY (phone call)
      │               Activate full IRT within 15 minutes
      │
      └─── P3/P4 ──▶ Email notification within 1 hour
                      Assign to on-call analyst
```

### 4.3 Incident Tracking

Every incident gets:
- **Incident ID:** INC-YYYY-NNNN (e.g., INC-2025-0047)
- **Severity:** P1–P4
- **Status:** Open → In Progress → Contained → Closed
- **Owner:** Primary analyst assigned
- **Timeline:** All actions logged with timestamps

---

## 5. Response Procedures by Phase

### 5.1 Phase 1 – Preparation (Ongoing)

**Monthly:**
- Review and update contact lists
- Test backup restoration (full restore test)
- Verify SIEM alert functionality
- Check IR tool availability (forensic kits, out-of-band comms)

**Quarterly:**
- Conduct tabletop exercises simulating realistic incident scenarios
- Review and update IR playbooks
- Test communication templates
- Assess IR team training needs

**Annually:**
- Full IR plan review and update
- Red team exercise
- Third-party IR assessment
- Update threat intelligence and attack scenarios

### 5.2 Phase 2 – Detection and Analysis

**Upon receiving an alert or report:**

```
Checklist — First 30 Minutes:
□ Document the initial report (who, what, when, where)
□ Assign Incident ID
□ Determine preliminary severity (P1–P4)
□ Notify appropriate team members
□ Open secure out-of-band communication channel
□ Begin evidence preservation BEFORE touching systems
□ Start incident timeline log

Evidence Preservation Order (most volatile first):
□ 1. RAM dump (running processes, encryption keys)
□ 2. Network connections (netstat, active sessions)
□ 3. Running processes (process list, open files)
□ 4. Log files (before they rotate)
□ 5. Disk images (forensic copy of storage)
□ 6. Cloud logs (CloudTrail, flow logs — export immediately)
```

### 5.3 Phase 3 – Containment, Eradication & Recovery

**Containment Decision Matrix:**

| Scenario | Immediate Action |
|----------|----------------|
| Ransomware spreading | Isolate ALL affected systems from network immediately |
| Single compromised host | Isolate from network, maintain for forensics |
| Compromised credentials | Disable account, force password reset across all systems |
| Data exfiltration in progress | Block destination IP at firewall, isolate source host |
| AWS compromise | Disable access keys, revoke sessions, enable CloudTrail |
| DDoS attack | Activate DDoS protection (Cloudflare/AWS Shield), rate limit |

**Eradication Checklist:**
```
□ Identify ALL affected systems (attackers have multiple footholds)
□ Remove malware and malicious files
□ Delete unauthorized user accounts and SSH keys
□ Remove persistence mechanisms (cron jobs, scheduled tasks, registry keys)
□ Close initial access vector (patch, revoke credentials)
□ Reset ALL passwords (not just compromised ones)
□ Rotate all API keys, certificates, secrets
□ Reimage compromised systems from clean baselines
□ Verify clean with EDR scan before returning to network
```

**Recovery Checklist:**
```
□ Restore from verified clean backups
□ Apply all security patches before reconnecting
□ Test restored systems thoroughly
□ Reconnect in priority order (Tier 1 → Tier 2 → Tier 3)
□ Monitor intensively for 72 hours post-recovery
□ Validate all business functions are operational
□ Confirm data integrity for customer-facing systems
```

### 5.4 Phase 4 – Post-Incident Activity

**Post-Incident Review (within 2 weeks):**

```
Agenda for PIR Meeting:
1. Incident timeline walkthrough (30 min)
2. Root cause analysis (20 min)
3. What went well? (15 min)
4. What went wrong / what was missed? (20 min)
5. Recommendations and action items (15 min)
6. Assign owners and deadlines for each action (10 min)
```

---

## 6. Communication Plan

### 6.1 Internal Communications

| Audience | When | Channel | Who |
|----------|------|---------|-----|
| IRT members | Immediately | Phone / Signal | Incident Commander |
| Executive team | Within 1 hour (P1/P2) | Phone + email | CISO |
| All employees | If systems affected | Internal email | Communications Lead |
| IT staff | During containment | Slack #incident-response | Technical Lead |

### 6.2 External Communications

| Audience | When | Who Approves |
|----------|------|-------------|
| Customers (PII breach) | Per regulatory requirement | Legal + CEO |
| Payment card brands (if PCI data) | Immediately | CISO + Legal |
| CERT-In (India) | Within 6 hours for critical incidents | CISO |
| Law enforcement (FBI/Police) | If criminal activity | Legal + CEO |
| Media / Press | Only if legally required | CEO + PR |

### 6.3 Communication Templates

**Customer Notification (Data Breach):**
```
Subject: Important Security Notice from ShopNow

Dear [Customer Name],

We are writing to inform you of a security incident that may have
affected your personal information stored with ShopNow.

WHAT HAPPENED: [Brief, factual description]
WHAT INFORMATION WAS INVOLVED: [Specific data types]
WHEN IT HAPPENED: [Date range]
WHAT WE ARE DOING: [Response actions taken]
WHAT YOU CAN DO: [Specific protective steps]

If you have questions, please contact: security@shopnow.com
or call our dedicated hotline: [number]

We sincerely apologize for any concern this may cause.
[CISO Name], Chief Information Security Officer
```

---

## 7. Legal and Regulatory Requirements

### 7.1 Notification Obligations

| Regulation | Trigger | Deadline | Authority |
|-----------|---------|----------|-----------|
| **IT Act 2000 (India)** | Any security breach | Reasonable time | CERT-In |
| **CERT-In Directions 2022** | Cyber incidents | 6 hours | CERT-In |
| **GDPR** (if EU customers) | PII breach | 72 hours | DPA |
| **PCI-DSS** | Card data breach | Immediately | Card brands |
| **RBI Guidelines** | Banking data | 2–6 hours | RBI CSITE |

### 7.2 Evidence Preservation for Legal

```
Legal Hold Requirements:
□ Do NOT delete or modify any logs or system files
□ Create forensic images before ANY remediation
□ Document chain of custody for all evidence
□ Store evidence in read-only, tamper-evident format
□ Maintain evidence for minimum 7 years
□ Engage legal counsel before sharing evidence externally
```

---

## 8. Metrics and KPIs

| Metric | Target | Measurement |
|--------|--------|-------------|
| Mean Time to Detect (MTTD) | < 1 hour | Detection time − attack start time |
| Mean Time to Contain (MTTC) | < 4 hours | Containment time − detection time |
| Mean Time to Recover (MTTR) | < 24 hours (P2), < 72 hours (P1) | Recovery time − detection time |
| False Positive Rate | < 5% | False alerts / total alerts |
| IR Plan Test Frequency | Quarterly | Tabletop exercises conducted |
| Patch SLA Compliance | > 95% | Critical CVEs patched within 48hrs |

---

*Document prepared as part of Cybersecurity Internship – Week 4, Task 1*
*Reference: NIST SP 800-61 Rev 2, ISO/IEC 27035, CERT-In Guidelines 2022*
