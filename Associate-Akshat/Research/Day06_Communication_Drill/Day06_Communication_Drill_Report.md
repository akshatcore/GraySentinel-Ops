# Day 06 — Communication & Escalation Drill Report
**Operator:** Akshat Tiwari (MTS, Intelligence Command)  
**Program:** GraySentinel ODP — Day 06  
**Date:** 2025  
**Reporting Manager:** Lavanya Agre  

---

## Section 1 — Research Answers

### Q1. Characteristics of a good incident notification message
A good incident notification message is:
- **Concise** — fits on a phone screen; no paragraphs
- **Time-stamped** — includes exact time of detection (not "just now")
- **Factual** — only confirmed observations, not assumptions
- **Action-oriented** — ends with what is being done next or what is needed
- **Severity-tagged** — clearly states P1/P2/P3 or CRITICAL/HIGH/LOW
- **Recipient-appropriate** — technical depth matches who is receiving it

### Q2. SBAR Framework for Security Escalations

| Component | Meaning | Security Example |
|---|---|---|
| **S — Situation** | What is happening right now? | "Student's system shows active file encryption" |
| **B — Background** | What context is relevant? | "Student is on GraySentinel lab network, Windows 10 VM" |
| **A — Assessment** | What do you think this is? | "Pattern consistent with ransomware or test encryption tool" |
| **R — Recommendation** | What action is needed? | "Isolate the machine, confirm lab vs production, escalate to Lavanya" |

SBAR was originally a medical handoff protocol (Kaiser Permanente, ~2002) and is now standard in incident response because it forces structured thinking under pressure.

### Q3. Mandatory information when escalating a possible breach to Lavanya

1. **Time of detection** (exact, not approximate)
2. **Reporter identity** — who reported, how, via which channel
3. **System/asset affected** — hostname, IP, OS
4. **Observable symptoms** — what was seen/heard (not interpreted)
5. **Scope estimate** — is it one system or multiple?
6. **Network segment** — is it lab or production?
7. **Immediate actions already taken** — what you did before escalating
8. **Your current assessment** — preliminary severity
9. **Next step you are requesting from Lavanya** — decision, resource, or approval

### Q4. Speed vs Accuracy when reporting

The correct balance follows a **two-phase model**:

**Phase 1 (0–5 min): Speed priority**  
Send an initial notification immediately — even with partial information. Delay costs containment time. Use the format: *"Possible incident detected. Investigating. Will update in 10 minutes."*

**Phase 2 (5–15 min): Accuracy priority**  
Send a structured SBAR update. Do not speculate — label unknowns as "unconfirmed." Accuracy matters more at this stage because leadership makes containment decisions based on it.

**Rule of thumb:** Never delay the first message more than 5 minutes waiting for complete information.

### Q5. Incident vs Event

| Term | Definition | Example |
|---|---|---|
| **Event** | Any observable occurrence in a system | A failed login attempt |
| **Incident** | An event that violates or threatens to violate security policy | 50 failed logins + successful auth = brute force incident |

All incidents are events. Not all events are incidents. The distinction determines whether escalation is required.

### Q6. Real-world case — Slow escalation causing a major breach

**Target Corporation Data Breach (2013)**  
Target's security team received automated alerts from their FireEye system on November 30, 2013, detecting malware in the POS environment. The alerts were reviewed but not escalated. Over the following two weeks, 40 million credit/debit card numbers were exfiltrated before Target's bank partners flagged the anomaly externally and forced action. The cost exceeded $200 million. Post-breach analysis showed the FireEye tool had correctly flagged the threat — the failure was entirely in human escalation and communication protocols.

**Lesson:** Automated detection without defined escalation thresholds and human ownership is equivalent to no detection.

### Q7. Template messages and stress reduction

Under stress, cognitive load spikes and recall drops. Template messages work because:
- They eliminate "what do I say first" paralysis
- They guarantee no critical field is forgotten
- They create consistent tone (calm, professional vs panicked)
- They speed up typing — fill blanks, not compose from scratch

GraySentinel should maintain **three core templates**: initial notification, SBAR structured update, and false-alarm stand-down.

### Q8. Open-source incident management platforms via GitHub Issues

GitHub Issues is highly viable as a lightweight ticketing system:
- **Labels** map to severity (P1-Critical, P2-High, P3-Low)
- **Assignees** map to ownership (IR lead, operations, documentation)
- **Milestones** map to incident phases (Detection, Containment, Eradication, Recovery)
- **Issue templates** enforce structured intake (summary, timeline, evidence links)
- **Comments** serve as a timestamped audit trail

Full platforms: **TheHive** (FOSS, purpose-built for IR), **FIR** (Fast Incident Response by CERT Societe Generale), **Wazuh** (includes case management). For GraySentinel's current scale, GitHub Issues + templates is the most practical starting point.

### Q9. Handling a critical incident when Lavanya is unavailable

**Protocol:**
1. Attempt Lavanya via primary channel (WhatsApp) — wait 2 minutes
2. Attempt Lavanya via secondary channel (call) — wait 2 minutes
3. Escalate to **Ritik Sir** (Operations Contact) — state that Lavanya is unreachable
4. Document all attempts with timestamps in the incident log
5. Continue initial containment actions within your authority (isolate, preserve, do not delete)
6. Do NOT wait on an unresponsive point of contact before starting containment

**Never**: let a critical incident sit idle waiting for one person.

### Q10. Dangers of over-escalating false positives

- **Alert fatigue** — team stops treating escalations seriously ("boy who cried wolf")
- **Credibility erosion** — analyst's future genuine escalations receive less urgency
- **Resource waste** — IR team mobilizes unnecessarily, disrupting other work
- **Desensitization** — the ops group starts ignoring high-priority notifications

**Mitigation**: Use a pre-escalation triage checklist (2–3 minute quick check) to confirm minimum evidence before escalating. Document why you escalated even if it turns out false — this shows process discipline.

### Q11. Non-technical details important for management

| Business Detail | Why Management Cares |
|---|---|
| Systems/services affected | Operational downtime, SLA impact |
| Student/data exposure risk | Liability, privacy obligations |
| Estimated recovery time | Resource planning |
| External reporting requirements | Legal/regulatory compliance |
| Reputational risk | Public communications needed? |
| Cost estimate | Budget authorization for response |

Management does not need to know which MITRE technique was used — they need to know what is broken and when it will be fixed.

### Q12. Communicating a false alarm without losing credibility

**Steps:**
1. Send a brief, professional stand-down message immediately
2. Briefly explain what triggered the alert and why it was a false positive
3. State what triage steps were completed before stand-down
4. Thank the team for responsiveness
5. Log the false positive as a data point to improve detection rules

**Never**: disappear silently, over-apologize, or blame the reporting tool.

**Example stand-down message:**
> ✅ STAND DOWN — DRILL COMPLETE  
> Investigation concluded. Reported file encryption was [cause — e.g., a student running VeraCrypt]. No malware detected. All systems nominal. Thanks for the rapid response. Incident log updated.

### Q13. Role of an "Operations Lead" during an incident

The operations lead (Ritik Sir in GraySentinel's case) is responsible for:
- **Coordination** — ensuring every team member knows their assigned role
- **Communication bridge** — between technical analysts and management
- **Scope control** — preventing scope creep or unauthorized actions
- **Timeline tracking** — maintaining the official incident log
- **Decision authority** — when the IR lead needs an operational call (isolate vs monitor)
- **Stakeholder updates** — regular status push to Lavanya and leadership

The operations lead does NOT do technical investigation — they manage the people doing it.

### Q14. WhatsApp polling feature for quick team decisions

WhatsApp polls (available in groups) allow:
- **Binary decisions** in <60 seconds (e.g., "Isolate the system now? YES / WAIT for more info")
- **Anonymous voting** — reduces social pressure to agree with seniors
- **Visible results** in real time — majority can be acted on immediately
- **Timestamped** — visible in message history for documentation

**Limitation**: Should not replace formal decision documentation. Poll results must be copied to the incident log.

### Q15. Draft escalation message — Student found a phishing page targeting GraySentinel

```
🚨 SECURITY ALERT — Phishing Page Detected
Time: [HH:MM, DD-MM-YYYY]
Reporter: [Student Name], GraySentinel Lab Batch

SITUATION: A phishing page impersonating GraySentinel has been identified.

URL: [FULL URL — DO NOT CLICK, screenshot attached]
Hosting: [If known — e.g., Cloudflare Pages, GitHub.io]
Target: Appears to mimic GraySentinel login / student portal
Discovery method: [How found — Google search / shared link]

SCREENSHOT: [Attached or link]
ACTIONS TAKEN: URL not clicked post-discovery. Page not reported to host yet — awaiting instruction.

RECOMMENDATION: Request Lavanya Ma'am / Ritik Sir to:
  1. Confirm takedown authority
  2. Submit abuse report to hosting provider
  3. Alert student batch to not interact with URL

Severity: P2 — HIGH
Awaiting direction.
— Akshat Tiwari, Intelligence Command
```

---

## Section 2 — GraySentinel Operations Drill After-Action Review

### 2.1 Drill Overview

| Parameter | Detail |
|---|---|
| **Drill Type** | Simulated ransomware report |
| **Trigger Message** | "My system is encrypting files! Ransomware!" |
| **Source** | Student in Operations group (simulated by Lavanya Ma'am) |
| **Drill End Point** | Triage phase — no actual malware involved |
| **Operator on duty** | Akshat Tiwari |

### 2.2 Simulated Timeline

| Time | Action | Notes |
|---|---|---|
| T+0:00 | Received alert in Operations WhatsApp group | Treated as real — escalation protocol initiated |
| T+0:45 | Replied with acknowledgement + request for system info | "Received. Do not touch the machine. Send hostname and IP immediately." |
| T+1:30 | Gathered initial data from student (hostname, OS, IP) | Confirmed system was on lab network segment |
| T+2:00 | Verified with student: is this a lab exercise or real activity? | Student confirmed: not intentionally running any tool |
| T+2:30 | Sent SBAR escalation to Lavanya Ma'am via WhatsApp | Full structured message — see below |
| T+3:00 | Advised student: do not shut down, do not delete files, disconnect from network | Preservation first principle |
| T+4:00 | Drill concluded by Lavanya Ma'am | Stand-down message sent to group |

### 2.3 Escalation Message Sent to Lavanya (Simulated)

```
🚨 ESCALATION — Possible Ransomware
Time: [T+2:30]

SITUATION: Student reported active file encryption on their system.
BACKGROUND: System is a Windows 10 VM on lab network (192.168.1.x segment).
  Hostname: STUDENT-VM-04 | IP: 192.168.1.25
  Student confirms no encryption tool intentionally running.
ASSESSMENT: Symptoms consistent with ransomware or misconfigured batch script.
  Cannot confirm malware without direct system access.
  Lab network — student data only, no production systems reachable.
RECOMMENDATION:
  1. Request permission to access system remotely for triage
  2. Advise whether to isolate from lab network immediately
  3. If confirmed malware: snapshot VM before any action

Current Status: Student advised to preserve system and disconnect NIC.
Awaiting your direction.

— Akshat Tiwari | MTS, Intelligence Command
```

### 2.4 What Went Well

- Immediate acknowledgement sent — no delay in initial response
- Student was given clear, calm instructions (do not panic, do not shut down)
- SBAR format maintained under simulated pressure
- Network segment confirmed before severity assessment (lab vs production check)
- Documentation of actions started at T+0

### 2.5 What Could Be Improved

- I initially forgot to ask for a screenshot of the encryption behavior — added this in the revised checklist
- The message to student was slightly too technical ("disconnect NIC") — needs simpler language for non-technical reporters
- Time from receipt to escalation to Lavanya was ~2.5 minutes — target is under 2 minutes for P1 scenarios

### 2.6 Lessons Learned

1. **First message to reporter ≠ first message to Lavanya.** Gather minimum facts (30–60 sec), then escalate. Don't escalate before you have hostname and IP.
2. **Calm language to students matters as much as accurate language to Lavanya.** Two different communication registers, two different purposes.
3. **Lab vs production check is a mandatory triage step** — it determines severity and response scope entirely.
4. **Template messages saved ~90 seconds** compared to composing from scratch.
5. **Drill is still documented as a real drill** — "simulated by Lavanya Ma'am" is noted in the log, not hidden.

---

## Section 3 — Lessons Learned Summary

| # | Lesson |
|---|---|
| 1 | SBAR is more than a format — it forces structured thinking before you type |
| 2 | Speed and accuracy are not opposites — initial notification fast, SBAR accurate |
| 3 | Escalation to Lavanya requires minimum 4 fields: time, asset, symptoms, your assessment |
| 4 | False alarms documented properly protect credibility — silence destroys it |
| 5 | Operations lead coordinates people, analysts investigate — do not conflate roles |
| 6 | Template messages are professional tools, not shortcuts |
| 7 | GitHub Issues + labels = viable lightweight incident tracker for GraySentinel |
| 8 | Target 2013 breach is the canonical example of detection-without-escalation failure |

---

*Report prepared by Akshat Tiwari | GraySentinel ODP Day 06*