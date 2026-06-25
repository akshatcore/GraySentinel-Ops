# Day 05 — Daily Report
**Program:** GraySentinel 30-Day Operator Development Program  
**Operator:** Akshat | MTS, Intelligence Command  
**Date:** 2026-06-25  
**Mentor:** Lavanya Agre, Lead Intel Analyst  

---

## Summary

Day 05 focused on command-line triage proficiency across Linux and Windows environments. The day covered live triage methodology, log parsing with grep/awk/sed, process-network correlation, PowerShell forensic queries, detection engineering with Sigma, and a threat hunting exercise.

---

## Tasks Completed

| # | Task | Status |
|---|------|--------|
| 1 | Research Questions 1–15 | ✅ Complete |
| 2 | Linux Practical Investigation (artifact sim + triage) | ✅ Complete |
| 3 | Windows Investigation (EID 1/3, base64 decode, timeline) | ✅ Complete |
| 4 | Threat Hunting Exercise (hypothesis + hunt narrative) | ✅ Complete |
| 5 | Detection Engineering — Sigma rule (EID 7045 / EID 13) | ✅ Complete |
| 6 | Content Creation — Top 15 grep/awk/sed cheat sheet | ✅ Complete |
| 7 | Excellence Challenge — PowerShell triage automation script | ✅ Complete |

---

## Deliverables Produced

- `Day05_CLI_Triage_Report.md` — Triage findings, Windows investigation, hunt narrative
- `Day05_Sigma_Service_Creation.yml` — Multi-branch Sigma rule with test cases
- `Day05_Log_Analysis_CheatSheet.md` — 15 one-liners with explanations
- `Day05_Daily_Report.md` — This file
- `Evidence/windows_ps_scripts.txt` — PowerShell triage automation script

---

## Key Learnings

**Linux triage:** The `/proc` filesystem provides a live, memory-mapped view of running processes that persists even if binaries are deleted from disk — critical for detecting fileless malware. Parent-child process relationships via `ps -o pid,ppid,cmd` are essential for attribution.

**Log parsing:** The combination of `grep -oE` (regex extraction) and `awk` field selection eliminates 90% of log noise in under a minute. The real skill is knowing which fields matter in each log format.

**Windows IR:** Base64-encoded PowerShell (`-EncodedCommand`) is one of the most common obfuscation techniques. Decoding with `[System.Text.Encoding]::Unicode.GetString(...)` is a mandatory muscle memory skill — Unicode, not UTF-8.

**Detection engineering:** Effective Sigma rules must match the actual log source schema. A common mistake is writing field names for the wrong log source (e.g., using Sysmon field names against Windows System Event Log). This rule covers both sources with separate branches.

**Threat hunting:** The 4625→4624→7045 sequence is a textbook credential spray → persistence chain. Hypothesis-driven hunting is more efficient than open-ended log browsing.

---

## Blockers / Issues

None. All tasks completed within session.

---

## Tomorrow's Focus (Day 06)

Per program schedule — await briefing from Lavanya.

---

*Submitted: 2026-06-25 | GraySentinel Intelligence Command*