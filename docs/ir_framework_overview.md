# Incident Response Frameworks – NIST vs SANS

**Author:** Harshit
**Task:** Week 4 – Task 1 | Cybersecurity Internship
**Date:** June 2025

---

## 1. Why IR Frameworks Matter

An Incident Response Framework provides a structured, repeatable methodology for handling security incidents. Without one, organizations waste critical time debating what to do next, miss important steps, and fail to learn from incidents.

The two most widely adopted frameworks are:
- **NIST SP 800-61 Rev 2** — US Government standard, comprehensive
- **SANS PICERL** — Practitioner-focused, more granular steps

---

## 2. NIST SP 800-61 Framework

Published by the National Institute of Standards and Technology, NIST SP 800-61 defines IR as a **4-phase continuous cycle**.

```
┌─────────────────────────────────────────────────────────┐
│                   NIST IR LIFECYCLE                     │
│                                                         │
│  ┌─────────────┐        ┌──────────────────────┐       │
│  │             │        │                      │       │
│  │ PREPARATION │───────▶│ DETECTION & ANALYSIS │       │
│  │  (Phase 1)  │        │     (Phase 2)        │       │
│  │             │        │                      │       │
│  └─────────────┘        └──────────┬───────────┘       │
│         ▲                          │                   │
│         │                          ▼                   │
│  ┌──────┴──────┐        ┌──────────────────────┐       │
│  │             │        │   CONTAINMENT,        │       │
│  │  POST-INC.  │◀───────│   ERADICATION &       │       │
│  │  ACTIVITY   │        │   RECOVERY            │       │
│  │  (Phase 4)  │        │   (Phase 3)           │       │
│  └─────────────┘        └──────────────────────┘       │
└─────────────────────────────────────────────────────────┘
```

### Phase 1 – Preparation
Build capability BEFORE incidents happen:
- Develop and test IR policies and procedures
- Build and train the Incident Response Team (IRT)
- Deploy detection tools (SIEM, IDS, EDR)
- Establish communication channels and contact lists
- Conduct tabletop exercises and drills

### Phase 2 – Detection and Analysis
- Identify that an incident has occurred
- Determine incident scope and severity
- Collect and preserve evidence
- Classify using severity matrix (P1–P4)
- Notify stakeholders based on severity

### Phase 3 – Containment, Eradication & Recovery
- **Containment:** Stop the spread (isolate systems, block IPs)
- **Eradication:** Remove all traces of the attacker
- **Recovery:** Restore systems and validate operations

### Phase 4 – Post-Incident Activity
- Conduct lessons-learned meeting within 2 weeks
- Document root cause, timeline, and improvements
- Update IR plan and detection rules
- Brief security team on new threat intelligence

---

## 3. SANS PICERL Framework

SANS adds more granularity with **6 phases**, splitting detection from analysis and adding explicit "Lessons Learned" as a phase.

```
P ── Preparation    → Build IR capability
I ── Identification → Detect + confirm incident
C ── Containment    → Stop the spread
E ── Eradication    → Remove threat completely
R ── Recovery       → Restore normal operations
L ── Lessons Learned → Improve for next time
```

---

## 4. Framework Comparison

| Aspect | NIST SP 800-61 | SANS PICERL |
|--------|---------------|-------------|
| Phases | 4 | 6 |
| Origin | US Government | Security practitioners |
| Focus | Policy + process | Tactical execution |
| Best for | Compliance, documentation | Hands-on IR teams |
| Lessons Learned | Part of Phase 4 | Explicit final phase |
| Industry adoption | Very high (global standard) | Very high (practitioner favorite) |
| Granularity | Medium | High |

---

## 5. MITRE ATT&CK Integration

Modern IR teams map incidents to the MITRE ATT&CK framework to understand **adversary tactics, techniques, and procedures (TTPs)**:

```
ATT&CK Tactic → IR Phase Mapping:

Reconnaissance        → Detection (Phase 2)
Initial Access        → Detection (Phase 2)
Execution             → Detection + Containment
Persistence           → Eradication (Phase 3)
Privilege Escalation  → Containment + Eradication
Lateral Movement      → Containment (Phase 3)
Collection            → Detection (Phase 2)
Exfiltration          → Containment (Phase 3)
Command & Control     → Containment + Eradication
Impact (Ransomware)   → All phases
```

Mapping to ATT&CK helps responders understand what the attacker is trying to achieve at each step and anticipate their next move.

---

*Reference: NIST SP 800-61 Rev 2 | SANS Incident Response Process | MITRE ATT&CK v14*
