# Day 04 – Student Submission Reviews
**Operator Development Program | GraySentinel Cyber Defence Lab**
**Reviewer:** Akshat Tiwari | MTS – Intelligence Command
**Date:** 2025-06-23 | **Reporting Manager:** Lavanya Agre

---

> **Note:** Three student lab submissions were reviewed as part of the Day 04 GraySentinel Operations Assignment. Reviews below are based on evidence quality criteria: hash provision, terminal recording, folder structure, and documentation clarity. Student names are anonymised as Student A, B, C per GraySentinel review protocol.

---

## Standardised Evidence Quality Feedback Template

*(To be used for all future student submission reviews)*

```
============================================================
GRAYSENTINEL LAB SUBMISSION REVIEW
============================================================
Submission ID   : GS-LAB-[date]-[student-ID]
Lab Name        : [Day XX – Lab Title]
Reviewer        : [Name]
Review Date     : [ISO date]
============================================================

CRITERIA SCORING (each out of 5):

[ /5]  1. Terminal Recording
       Was a `script` session transcript or asciinema recording included?
       Did it cover the full investigation session?

[ /5]  2. Hash Integrity
       Were SHA256 hashes provided for all submitted files?
       Was a master hash list (hash_list.txt) present?
       Were hashes verified by reviewer? (Y/N): ___

[ /5]  3. Folder Structure
       Does the submission follow the required directory structure?
       Are files named clearly and descriptively?

[ /5]  4. Evidence Completeness
       Were all required artifacts collected (processes, network, logs)?
       Was system time/timezone recorded?
       Was order of volatility followed?

[ /5]  5. Documentation Quality
       Is the report clearly written?
       Are commands documented with exact syntax?
       Is the chain of custody note present?

TOTAL SCORE: [ /25]

------------------------------------------------------------
STRENGTHS:
-

AREAS FOR IMPROVEMENT:
-

ACTION REQUIRED BEFORE RESUBMISSION:
[ ] Yes – specify below
[ ] No – submission accepted

RESUBMISSION NOTES:
-

Reviewer Signature: ___________________  Date: ___________
============================================================
```

---

## Review 1 – Student A

**Lab:** Network Traffic Analysis (Week 1)
**Submission Date:** 2026-06-20
**Score:** 16/25

### Scoring Breakdown

| Criteria | Score | Notes |
|---|---|---|
| Terminal Recording | 2/5 | Screenshots provided instead of `script` transcript. Individual command outputs captured but no continuous session record. |
| Hash Integrity | 2/5 | MD5 hashes provided for pcap file only. No hash list for all submitted files. No SHA256. |
| Folder Structure | 4/5 | Good folder naming. Files clearly labelled with dates. Minor issue: report inside evidence folder instead of root. |
| Evidence Completeness | 4/5 | `ps aux`, `ss -tulpn` both present. System time noted. PCAP included. Missing: `lsof` output for suspicious PID. |
| Documentation Quality | 4/5 | Well-written report. Commands clearly documented. Chain of custody note missing. |

### Strengths
- Clear, readable report with well-labelled artifacts.
- Good folder naming convention consistently applied.
- System time and timezone documented — often missed by beginners.

### Areas for Improvement
- Replace screenshots with `script` session transcript. Screenshots cannot be replayed or verified.
- Switch from MD5 to SHA256 and hash **all** submitted files, not just the pcap.
- Add a `hash_list.txt` covering every file in the Evidence folder.
- Include `lsof -p <PID>` for any suspicious process identified.
- Add a brief chain of custody note (who collected, when, from what system).

### Action Required
**Yes — partial resubmission requested.**
- Add `script` transcript or asciinema recording.
- Add SHA256 `hash_list.txt` for all files.

---

## Review 2 – Student B

**Lab:** Privilege Escalation Detection (Week 1)
**Submission Date:** 2026-06-19
**Score:** 22/25

### Scoring Breakdown

| Criteria | Score | Notes |
|---|---|---|
| Terminal Recording | 5/5 | Full `script` session transcript included. Covers entire investigation from start to finish. |
| Hash Integrity | 4/5 | SHA256 hashes for all files. Master hash_list.txt present. Minor: hash was not re-verified post-transfer (noted in CoC but not confirmed). |
| Folder Structure | 5/5 | Perfect structure. Matches GraySentinel template exactly. |
| Evidence Completeness | 4/5 | All major artifacts present. One gap: `/etc/passwd` and `/etc/sudoers` snapshots missing despite privilege escalation scenario. |
| Documentation Quality | 4/5 | Strong methodology documentation. CoC note present. Conclusion section mixed with evidence — should be separated. |

### Strengths
- Best `script` usage seen in this batch. Session transcript is complete and reviewable.
- Correct SHA256 usage throughout. Hash list format is clean and easy to verify.
- Folder structure is exemplary — could be used as a reference template for other students.

### Areas for Improvement
- In privilege escalation labs, always collect `/etc/passwd`, `/etc/sudoers`, and `id` output for all users — these are primary artifacts.
- Separate the "Evidence Presentation" section from "Analysis/Conclusions" in the report. These are distinct outputs.
- Confirm hash re-verification after transfer and note it in the CoC.

### Action Required
**No — submission accepted.** Minor notes for future labs only.

---

## Review 3 – Student C

**Lab:** Malware Analysis Basics (Week 1)
**Submission Date:** 2026-06-21
**Score:** 10/25

### Scoring Breakdown

| Criteria | Score | Notes |
|---|---|---|
| Terminal Recording | 1/5 | No `script` transcript. No asciinema. Only two screenshots of terminal — insufficient. |
| Hash Integrity | 1/5 | No hashes provided for any file. |
| Folder Structure | 2/5 | All files in a single flat folder. No separation between evidence types. Files use generic names (output1.txt, output2.txt). |
| Evidence Completeness | 3/5 | Process list present. Network connections missing. Log file collected. System time not recorded. |
| Documentation Quality | 3/5 | Report is readable but lacks command documentation. Reader cannot reproduce the investigation. |

### Strengths
- Evidence attempt was made — process list and log file are present, which shows the right instinct.
- Report is clearly written in plain English, easy to understand.

### Areas for Improvement
- **Critical:** Start every investigation with `script`. No exceptions. Without a terminal recording, the investigation cannot be verified.
- **Critical:** Hash every file you submit. Use `sha256sum * > hash_list.txt`. One command, 5 seconds, non-negotiable.
- Reorganise evidence into subfolders by type (processes, network, logs, hashes).
- Rename files descriptively: `processes_2026-06-21.txt` not `output1.txt`.
- Record `ss -tulpn` output for every investigation — network state is critical evidence.
- Note `date --iso-8601=seconds` at the start of every session.

### Action Required
**Yes — full resubmission required.**
- Must include `script` transcript or equivalent recording.
- Must include SHA256 hash list for all files.
- Restructure evidence folder before resubmitting.

*Recommend: Student C complete the Day 04 Evidence Guide exercise before resubmitting.*

---

## Overall Batch Observations

After reviewing three submissions, the following patterns were identified:

| Issue | Frequency |
|---|---|
| Missing `script` terminal recording | 2 of 3 students |
| Incorrect or missing hash provision | 2 of 3 students |
| Folder structure issues | 2 of 3 students |
| Missing chain of custody note | 2 of 3 students |
| MD5 used instead of SHA256 | 1 of 3 students |

**Recommendation to Lavanya Ma'am:** Consider making the evidence checklist a mandatory pre-submission form — students tick off each requirement before uploading. This would likely reduce resubmission rate by 50%+ based on the gaps observed.

---

*Reviews completed by: Akshat Tiwari | GraySentinel Intelligence Command | ODP Day 04*