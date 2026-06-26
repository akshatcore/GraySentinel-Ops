# 🚨 GraySentinel — Incident Escalation Cheat Sheet
**Intelligence Command | ODP Reference Card**

---

## STEP 1 — IS THIS AN INCIDENT?

| Signal | Escalate? |
|---|---|
| Single failed login | ❌ Log it |
| 5+ failed logins on same account | ✅ Monitor + note |
| Successful login after failures | 🚨 ESCALATE |
| File encryption / mass deletion | 🚨 ESCALATE NOW |
| Phishing page targeting GraySentinel | 🚨 ESCALATE |
| Student reports "something weird" | ✅ Triage first, 2 min |
| System unreachable / offline | ✅ Check + escalate if unexplained |

---

## STEP 2 — TRIAGE FIRST (2 MINUTES MAX)

Before you escalate, grab:
- [ ] Time of detection
- [ ] Reporter name + contact
- [ ] Hostname + IP address
- [ ] Lab or production?
- [ ] What exactly was observed (screenshot if possible)
- [ ] Any actions already taken?

---

## STEP 3 — WHO TO CONTACT

| Scenario | Contact | Channel |
|---|---|---|
| Any incident, Lavanya available | **Lavanya Agre** | WhatsApp (first) → Call |
| Lavanya unreachable >2 min | **Ritik Sir** | WhatsApp → Call |
| Both unreachable, critical incident | Escalate to both + act within authority | Document attempts with timestamps |

---

## STEP 4 — WHATSAPP ESCALATION TEMPLATE

```
🚨 [SEVERITY] — [Incident Type]
Time: HH:MM | DD-MM-YYYY

SITUATION: [1 sentence — what is happening]
BACKGROUND: Asset: [hostname/IP] | OS: [OS] | Location: [lab/production]
ASSESSMENT: [Your preliminary read — be honest about unknowns]
RECOMMENDATION: [What you need from Lavanya — decision / resource / approval]

Actions taken so far: [List]
Awaiting direction.
— [Your name], Intelligence Command
```

---

## STEP 5 — SEVERITY GUIDE

| Level | Label | Criteria | Response Time |
|---|---|---|---|
| P1 | 🔴 CRITICAL | Production system / data breach / active ransomware | Escalate in <2 min |
| P2 | 🟠 HIGH | Lab compromise / phishing page / suspected breach | Escalate in <5 min |
| P3 | 🟡 MEDIUM | Suspicious activity, unconfirmed, lab only | Escalate within 15 min |
| P4 | 🟢 LOW | Anomaly, no impact, informational | Log + mention in daily report |

---

## FALSE ALARM STAND-DOWN TEMPLATE

```
✅ STAND DOWN — [Time]
Investigation complete. Alert was: [explanation in 1 sentence]
No malicious activity confirmed. Systems normal.
Actions taken: [brief list]
Incident log updated.
— [Your name]
```

---

## GOLDEN RULES

1. **Never delay the first message >5 min** waiting for complete info
2. **Lab vs production** check is MANDATORY before assigning severity
3. **Tell student: don't shut down, don't delete, disconnect from network**
4. **Document every action with timestamp** as you go — not after
5. **One false alarm with documented process > silence**

---

*GraySentinel Intelligence Command | Akshat Tiwari | ODP Day 06*