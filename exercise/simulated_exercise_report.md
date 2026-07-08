# Simulated Incident Response Exercise Report

**Exercise Type:** Tabletop Exercise (TTX)
**Scenario:** Advanced Persistent Threat (APT) — Data Breach + Ransomware
**Organization:** ShopNow E-Commerce Platform (Hypothetical)
**Author:** Harshit
**Task:** Week 4 – Task 1 | Cybersecurity Internship
**Exercise Date:** June 2025
**Duration:** 3 hours (simulated)

---

## 1. Exercise Overview

### 1.1 Purpose

This tabletop exercise simulates a multi-stage cyberattack against ShopNow, requiring participants to apply the Incident Response Plan in a realistic scenario. The exercise tests:
- Team coordination and communication under pressure
- Decision-making at each IR phase
- Knowledge of regulatory notification requirements
- Technical response capabilities

### 1.2 Participants (Hypothetical Roles)

| Role | Participant |
|------|------------|
| Incident Commander | CISO |
| Technical Lead | Lead Security Analyst |
| Forensics | Security Analyst |
| IT Operations | IT Manager |
| Legal | General Counsel |
| Communications | Marketing Director |
| Executive Sponsor | CEO |
| Intern Observer | Harshit (MCA Cybersecurity) |

### 1.3 Exercise Rules

- No actual systems will be touched
- All actions are discussed and documented
- Decisions made must reference the IR plan
- Time pressure is simulated — each inject has a response deadline
- Facilitator introduces new information ("injects") as the exercise progresses

---

## 2. Scenario Background

**Organization Context:**
ShopNow processes approximately 50,000 orders per day, stores payment card data for 500,000 customers, and employs 120 staff across offices in Bangalore and Mumbai. All production infrastructure runs on AWS (Mumbai region).

**Initial Situation Brief (provided to participants):**

> "It is Monday, 7:30 AM. The IT helpdesk starts receiving calls from employees unable to open files — they see a `.locked` extension on everything and a file called `HOW_TO_DECRYPT.txt` on their desktops. Simultaneously, the SIEM generates a critical alert for large outbound data transfer (450GB) that occurred overnight. The on-call analyst escalates to the security team."

---

## 3. Exercise Timeline and Injects

---

### INJECT 1 — 07:30 AM: Initial Discovery
**Situation:** Ransomware notes found on 8 employee workstations. Files encrypted. Large data transfer detected overnight to IP 91.108.4.200.

**Discussion Questions:**
1. What is your immediate first action?
2. Who do you notify and in what order?
3. Do you shut everything down immediately?

**Team Response:**
```
✅ Incident Commander declared P1 incident immediately
✅ Full IRT assembled via phone within 12 minutes
✅ Decision: Do NOT shut down yet — preserve forensic evidence
✅ SIEM queried for scope: 8 workstations confirmed, servers status unknown
✅ Incident ID INC-2025-0012 opened in tracking system
⚠️  Team initially forgot to preserve RAM before any actions — corrected after reminder
```

**Facilitator Evaluation:** Good initial response. RAM preservation reminder was important — this is a common real-world mistake that destroys valuable forensic evidence including encryption keys.

---

### INJECT 2 — 07:55 AM: Scope Expansion
**New Information:** The database server is not responding. CloudTrail shows 450GB transferred from S3 bucket `shopnow-customer-data` to an unknown AWS account at 02:00–04:00 AM. The customer PII database appears to have been exfiltrated.

**Discussion Questions:**
1. Has the severity level changed?
2. When do you notify customers?
3. What are your legal obligations now?

**Team Response:**
```
✅ Severity confirmed as P1 CRITICAL (data breach + ransomware)
✅ Legal counsel immediately briefed on PII exfiltration
✅ Legal identified: CERT-In notification required within 6 hours (CERT-In Directions 2022)
✅ PCI-DSS obligation identified: payment card brands must be notified
✅ Decision: Customer notification NOT yet — need to confirm scope first
✅ AWS Support Enterprise contacted for CloudTrail forensics assistance
⚠️  Team debated for 8 minutes on customer notification timing — lost critical time
```

**Facilitator Evaluation:** Legal response was strong. The notification timing debate highlighted a gap — the IR plan should have pre-defined notification triggers to avoid real-time debate.

---

### INJECT 3 — 08:20 AM: Attack Vector Identified
**New Information:** Forensic analysis of logs shows the initial access occurred 9 days ago via a phishing email to `accounts@shopnow.com`. The attacker harvested credentials, moved laterally over several days, and deployed ransomware only after completing data exfiltration.

**Discussion Questions:**
1. How does knowing the initial vector change your response?
2. Are there other accounts that may be compromised?
3. How do you contain without tipping off the attacker (too late, but practice the principle)?

**Team Response:**
```
✅ Email security logs reviewed — identified original phishing email
✅ Decision: Reset ALL user passwords company-wide, not just affected accounts
✅ MFA enforced immediately for all users
✅ All sessions invalidated across AWS, email, and VPN
✅ Searched email logs for other users who received same phishing email — found 3 more
✅ Accounts payable team briefed (CFO approval for emergency MFA deployment)
✅ IT identified 9-day dwell time meant attacker likely had full network map
⚠️  Team did not initially consider that attacker may still have active sessions
```

**Facilitator Evaluation:** Excellent lateral thinking on the 9-day dwell time. The realization that the attacker had 9 days to explore the network before anyone noticed is a key lesson — detection was far too slow.

---

### INJECT 4 — 09:00 AM: Containment Decisions
**New Information:** IT Operations has isolated 8 infected workstations. The database server is encrypted but the backup server appears unaffected. IT confirms backups are available from 48 hours ago.

**Discussion Questions:**
1. Do you pay the ransom?
2. What data is lost in the 48-hour backup gap?
3. How do you prioritize recovery?

**Team Response:**
```
✅ Decision: DO NOT pay ransom — backups available
✅ Legal confirmed: paying ransom may violate OFAC sanctions (ransomware group may be sanctioned)
✅ IT began restoring database server from 48-hour backup
✅ Recovery priority defined:
   Tier 1: Payment processing + customer database (revenue critical)
   Tier 2: Web application + order management
   Tier 3: Employee workstations
✅ 48-hour data loss quantified: ~100,000 orders — finance team briefed
✅ Insurance company notified (cyber insurance policy triggered)
⚠️  Team initially forgot to check if backup server was also compromised before restoring from it
```

**Facilitator Evaluation:** Critical catch on backup server integrity — this is exactly how ransomware operators defeat recovery plans. Always verify backup integrity before restoring.

---

### INJECT 5 — 10:15 AM: Communication Crisis
**New Information:** A journalist from a national newspaper calls the Communications Lead saying they've received an anonymous tip about a major data breach at ShopNow and are publishing a story in 2 hours.

**Discussion Questions:**
1. Do you confirm or deny?
2. Do you proactively notify customers before the article publishes?
3. Who is authorized to speak to media?

**Team Response:**
```
✅ CEO and Legal immediately involved
✅ Decision: No comment to media until legal approves — standard holding statement issued
✅ Communications prepared customer notification email for immediate send if legal approves
✅ Legal reviewed notification — approved sending to customers simultaneously with forced disclosure
✅ CEO signed off on customer notification
✅ Holding statement: "ShopNow is aware of a cybersecurity incident and is working with law enforcement and security experts. We will provide updates as information becomes available."
⚠️  Team did not initially consider the reputational benefit of proactively notifying customers before the article — being transparent first reduces reputational damage
```

**Facilitator Evaluation:** The media inject tests crisis communication under time pressure. The best practice is to control the narrative by notifying customers proactively — reactive disclosure always looks worse.

---

### INJECT 6 — 11:30 AM: Recovery Milestone
**New Information:** Database server restored from backup. Payment processing is back online. Ransomware has been eradicated from all identified systems. CERT-In notified at 12:30 PM (5 hours after discovery — within the 6-hour window).

**Discussion Questions:**
1. When do you declare the incident "closed"?
2. What monitoring should you put in place?
3. When can employees return to their workstations?

**Team Response:**
```
✅ Decision: Incident NOT closed — monitoring period of 72 hours before closure
✅ Enhanced monitoring deployed: all authentication events, unusual process creation, outbound traffic
✅ Employee workstations reimaged from clean baseline (not restored — too risky)
✅ New phishing awareness training scheduled for all staff
✅ External penetration test commissioned to find any remaining footholds
✅ Payment card brands notified (PCI-DSS obligation)
✅ Post-incident review scheduled for 7 days out
```

---

## 4. Exercise Results and Scoring

### 4.1 Performance Assessment

| IR Phase | Performance | Score | Notes |
|----------|-------------|-------|-------|
| Preparation | Good | 7/10 | IR plan existed but lacked pre-defined notification triggers |
| Detection | Poor | 4/10 | 9-day dwell time undetected — major failure |
| Analysis | Good | 8/10 | Scope accurately determined within 2 hours |
| Containment | Very Good | 9/10 | Fast isolation, good decisions |
| Eradication | Good | 8/10 | Thorough, but backup integrity check almost missed |
| Recovery | Good | 8/10 | Correct prioritization, 48-hr backup gap accepted |
| Communication | Fair | 6/10 | Notification timing debate caused delays |
| **Overall** | **Good** | **7.1/10** | |

### 4.2 What the Team Did Well

```
✅ IRT assembled quickly (12 minutes) — excellent
✅ Legal was engaged immediately — critical for breach notifications
✅ Decided NOT to pay ransom — correct decision
✅ Recovery prioritization was logical and business-aware
✅ CERT-In notified within 6-hour window — compliance maintained
✅ CEO-level decision on customer notification — appropriate escalation
✅ Catching backup server integrity issue before restoring — excellent catch
```

### 4.3 Gaps Identified

```
❌ 9-day dwell time not detected — SIEM/EDR coverage is insufficient
❌ RAM not preserved before initial actions — evidence destroyed
❌ 8-minute notification timing debate — pre-defined triggers needed
❌ Active attacker sessions not considered initially
❌ Media response was reactive rather than proactive
❌ No pre-approved customer notification template — had to draft under pressure
```

---

## 5. Lessons Learned

### Lesson 1: Detection is the Critical Failure Point
An attacker was inside the network for **9 days** without detection. This is the most important finding. Investments in SIEM tuning, EDR behavioral analytics, and network monitoring would have detected the lateral movement days before ransomware deployed.

**Action:** Deploy user behavior analytics (UBA), tune SIEM rules for lateral movement, implement network micro-segmentation.

### Lesson 2: Pre-Define Notification Triggers
The team spent 8 minutes debating when to notify customers — time that cannot be afforded in a real breach. The IR plan must specify **exact triggers** for each notification type.

**Action:** Update IR plan with decision tree: "IF PII confirmed exfiltrated THEN notify customers within X hours, regardless of investigation status."

### Lesson 3: RAM Preservation is Non-Negotiable
The team almost missed preserving RAM before touching infected systems. RAM contains critical forensic evidence — running processes, network connections, and potentially encryption keys.

**Action:** Add RAM preservation as the FIRST item in every containment checklist, before any other action.

### Lesson 4: Always Verify Backup Integrity Before Restoring
The team nearly restored from a backup server that could have been compromised. In ransomware attacks, attackers often deliberately corrupt or encrypt backups first.

**Action:** Add backup integrity verification (hash check + air-gap verification) as a mandatory pre-restoration step.

### Lesson 5: Proactive Communication Controls the Narrative
Waiting for legal to approve customer notification while a journalist was about to publish put the team in a reactive position. Customers who learn about a breach from the news are far more likely to lose trust.

**Action:** Prepare pre-approved customer notification templates. Define a "disclosure threshold" in the IR plan that triggers automatic notification without real-time legal debate.

---

## 6. Action Items from Exercise

| # | Action Item | Owner | Deadline | Priority |
|---|------------|-------|----------|---------|
| 1 | Deploy EDR on all 120 endpoints | IT Manager | 2 weeks | P0 |
| 2 | Implement UBA in SIEM for lateral movement detection | Security Lead | 1 month | P0 |
| 3 | Add RAM preservation as first containment step in all playbooks | Security Analyst | 1 week | P0 |
| 4 | Define pre-approved notification triggers in IR plan | CISO + Legal | 2 weeks | P1 |
| 5 | Create pre-approved customer notification template | Legal + Comms | 1 week | P1 |
| 6 | Test backup restoration monthly (not quarterly) | IT Operations | Ongoing | P1 |
| 7 | Implement phishing-resistant MFA company-wide | IT Manager | 1 month | P1 |
| 8 | Schedule next tabletop exercise in 90 days | CISO | 3 months | P2 |

---

## 7. Exercise Facilitator Notes

This tabletop exercise successfully identified 5 significant gaps in ShopNow's IR capability. The most critical finding — the 9-day undetected dwell time — reflects a real-world problem where most organizations detect breaches from external notification rather than internal detection.

The exercise also demonstrated that IR is not purely a technical challenge. Communication decisions, legal obligations, and business continuity planning all required equal attention during the simulated crisis. Future exercises should include more complexity around regulatory notifications and media management.

**Next Exercise Recommendation:** Red team simulation with actual technical execution on isolated test environment.

---

*Exercise report prepared as part of Cybersecurity Internship – Week 4, Task 1*
*Reference: NIST SP 800-84 (Guide to Test, Training, and Exercise Programs for IT Plans and Capabilities)*
