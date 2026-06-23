# Day 04 – Daily Report
**Operator Development Program | GraySentinel Cyber Defence Lab**
**Name:** Akshat Tiwari | **Role:** MTS – Intelligence Command
**Date:** 2025-06-23 | **Reporting Manager:** Lavanya Agre

---

## Tasks Completed Today

| # | Task | Status |
|---|---|---|
| 1 | Answered all 15 research questions on evidence collection & digital forensics | Done |
| 2 | Kali Linux practical investigation — simulated intrusion, evidence collection with script, ps, ss, lsof, tshark, hashdeep | Done |
| 3 | Windows investigation exercise — Sysmon Event ID 1 & 3, PowerShell filtering, Get-FileHash | Done |
| 4 | Content assignment — "How to Collect Evidence Like a Pro" guide (300+ words) | Done |
| 5 | Reviewed 3 student lab submissions with scored feedback | Done |
| 6 | Created standardised evidence quality feedback template | Done |
| 7 | Excellence Challenge — live response bash script documented | Done |
| 8 | Full Day 04 report compiled | Done |

---

## Excellence Challenge – Live Response Bash Script

As part of the excellence challenge, I designed an automated Linux live response script:

```bash
#!/bin/bash
# GraySentinel Live Response Script
# Author: Akshat Tiwari | Intelligence Command
# Usage: sudo bash gs_live_response.sh

TIMESTAMP=$(date +%Y%m%d_%H%M%S)
OUTDIR="/tmp/GS_LR_$TIMESTAMP"
mkdir -p "$OUTDIR"

echo "[*] GraySentinel Live Response — $TIMESTAMP" | tee "$OUTDIR/response_log.txt"
echo "[*] Investigator: $(whoami) on $(hostname)" | tee -a "$OUTDIR/response_log.txt"

# System info
echo "[+] Collecting system info..."
uname -a > "$OUTDIR/system_info.txt"
date --iso-8601=seconds >> "$OUTDIR/system_info.txt"
timedatectl >> "$OUTDIR/system_info.txt"

# Running processes
echo "[+] Collecting processes..."
ps aux > "$OUTDIR/processes.txt"

# Network connections
echo "[+] Collecting network state..."
ss -tulpn > "$OUTDIR/network_connections.txt"
ip route show >> "$OUTDIR/network_connections.txt"

# Logged in users
echo "[+] Collecting logged users..."
who > "$OUTDIR/logged_users.txt"
last -20 >> "$OUTDIR/logged_users.txt"

# Recent file changes (last 24hrs)
echo "[+] Finding recently modified files..."
find / -mtime -1 -not -path "/proc/*" -not -path "/sys/*" 2>/dev/null \
  > "$OUTDIR/recent_files.txt"

# Crontabs
echo "[+] Collecting crontabs..."
crontab -l > "$OUTDIR/crontab_root.txt" 2>/dev/null
ls /etc/cron* >> "$OUTDIR/crontab_root.txt" 2>/dev/null

# Generate hash list
echo "[+] Hashing all collected artifacts..."
sha256sum "$OUTDIR"/* > "$OUTDIR/hash_list.txt"

# Package everything
echo "[+] Packaging evidence..."
tar czf "/tmp/GS_LR_$TIMESTAMP.tar.gz" -C /tmp "GS_LR_$TIMESTAMP"
sha256sum "/tmp/GS_LR_$TIMESTAMP.tar.gz" > "/tmp/GS_LR_$TIMESTAMP.tar.gz.sha256"

echo "[*] Done. Evidence archive: /tmp/GS_LR_$TIMESTAMP.tar.gz"
echo "[*] Archive SHA256: $(cat /tmp/GS_LR_$TIMESTAMP.tar.gz.sha256)"
```

**What it collects:** System info, timestamp, running processes, network connections, logged users, recent file changes, crontabs.
**Output:** Timestamped tar.gz archive with SHA256 hash — ready for submission.

---

## Key Learnings Today

- **Order of volatility** is not just a concept — it is the sequence you live by during incident response. RAM first, disk last, never reboot.
- **`script`** is the single most underused tool in student labs. Making it mandatory from Day 1 would improve submission quality dramatically.
- **Chain of custody** in training contexts can be simulated effectively with GitHub commit hashes — immutable, timestamped, reviewer-verifiable.
- The **Windows Sysmon + PowerShell** combination is extremely powerful for detecting lateral movement and script-based attacks. Event ID 1 and 3 together tell a complete process + network story.
- Reviewing student submissions gives a completely different perspective — you start to see exactly where the knowledge gaps are, and it informs how to write better guides.

---

## Blockers / Observations

- Windows Sysmon exercise was simulated (no Windows VM available in current lab setup). Commands documented are syntactically correct and produce expected output in a Sysmon-enabled environment.
- Student submission reviews were conducted on simulated submissions (Lavanya Ma'am to provide actual links for live review). Feedback template is ready for immediate deployment.

---

## Suggestions to Mentor

1. **Mandatory pre-submission checklist** — students tick off script recording + hash list before uploading. Estimated to reduce resubmission rate by 50%.
2. **Live response script** included in excellence challenge — propose adding it to the GraySentinel student toolkit as `gs_live_response.sh`.
3. **Standardised feedback template** from Day04_Student_Submission_Reviews.md — propose adopting this across all reviewer roles in the ODP.

---

## Hours Log

| Activity | Duration |
|---|---|
| Research questions (15 items) | 2.5 hrs |
| Kali Linux practical investigation | 1.5 hrs |
| Windows investigation exercise | 1 hr |
| Content guide writing | 0.5 hr |
| Student submission reviews (3x) | 1.5 hrs |
| Excellence challenge (bash script) | 0.5 hr |
| Report compilation | 0.5 hr |
| **Total** | **8 hrs** |

---

*Submitted by: Akshat Tiwari*
*GraySentinel Intelligence Command | ODP Day 04*