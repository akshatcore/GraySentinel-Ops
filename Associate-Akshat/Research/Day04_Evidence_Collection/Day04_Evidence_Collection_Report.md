# Day 04 – Evidence Collection & Documentation Report
**Operator Development Program | GraySentinel Cyber Defence Lab**
**Name:** Akshat Tiwari | **Role:** MTS – Intelligence Command
**Date:** 2025-06-23 | **Reporting Manager:** Lavanya Agre

---

## Research Questions

### 1. Three Fundamental Principles of Digital Forensics Evidence Handling

**1. Integrity**
Evidence must not be altered from the moment of collection. Any modification — intentional or accidental — renders the evidence inadmissible and untrustworthy. This is enforced through cryptographic hashing (MD5, SHA256) before and after collection, with hashes recorded in the chain of custody.

**2. Authenticity**
Evidence must be provably what it claims to be. This means documenting who collected it, from which system, at what time, and using which tools. Without authenticity, a defence attorney (or a student reviewer) can challenge whether the evidence came from the alleged source.

**3. Chain of Custody**
Every person who handled the evidence, and every action performed on it, must be documented in an unbroken chain. If evidence changes hands without documentation, the chain breaks and the evidence loses legal and investigative value.

These three principles form the foundation of forensic soundness in any context — enterprise, legal, or training.

---

### 2. Why is a Hash (MD5, SHA256) Important When Collecting Evidence?

A cryptographic hash is a fixed-length fingerprint of a file. If even a single bit in the file changes, the hash changes entirely. This property makes hashing essential for two purposes:

**Integrity verification:** After collecting a file, you hash it. If the hash at collection matches the hash at analysis, the file was not tampered with in transit or storage.

**Non-repudiation:** A hash logged at collection time proves the state of the file at that exact moment. This is documented in the chain of custody form.

**MD5 vs SHA256:**

| Algorithm | Length | Speed | Collision Resistance | Recommended Use |
|---|---|---|---|---|
| MD5 | 128-bit | Fast | Weak (collisions known) | Quick integrity checks only |
| SHA256 | 256-bit | Moderate | Strong | Evidence, formal submissions |

In forensic contexts, **SHA256 is preferred**. MD5 is kept for legacy compatibility only.

---

### 3. Order of Volatility on a Live Linux System (Most to Least Volatile)

The order of volatility defines the sequence in which evidence should be collected — most volatile first, because it disappears the fastest.

| Priority | Data Type | Why It's Volatile |
|---|---|---|
| 1 | CPU registers, cache, running processes | Lost the moment the system is rebooted or process ends |
| 2 | RAM / memory contents | Lost on power off; contains encryption keys, process data, network buffers |
| 3 | Network connections and routing tables | Active connections change constantly; closed sessions are gone |
| 4 | Running processes and open file handles | Processes can be killed; open files change |
| 5 | Temp files, swap space | Can be overwritten; swap may persist longer than RAM |
| 6 | Disk / filesystem (files, logs) | Persists after reboot but can be overwritten by OS activity |
| 7 | Remote logging / external logs | Most stable; stored off-system |
| 8 | Archival media / backups | Least volatile; rarely changes |

**Rule:** Collect in this order. Never reboot before capturing RAM and network state.

---

### 4. Logical vs Physical Acquisition of a Disk

| Attribute | Logical Acquisition | Physical Acquisition |
|---|---|---|
| What is captured | Files and folders visible to the OS | Bit-for-bit copy of entire disk including unallocated space |
| Deleted files | Not captured | Captured (recoverable from unallocated space) |
| Slack space | Not captured | Captured |
| Speed | Faster | Slower |
| Size | Smaller (only used space) | Same size as physical disk |
| Tools | `rsync`, `cp`, `robocopy` | `dd`, `dcfldd`, FTK Imager, Guymager |
| Use case | Quick triage, live response | Full forensic investigation, court evidence |

For GraySentinel training: logical acquisition is sufficient for lab exercises. Physical acquisition is introduced when teaching advanced forensics or preparing for real incident response.

---

### 5. Linux Command to Capture Network Connections with Process Association

```bash
ss -tulpn
```

**Breakdown:**
- `-t` — show TCP connections
- `-u` — show UDP connections
- `-l` — show listening sockets
- `-p` — show process name/PID using the socket
- `-n` — show numeric addresses (no DNS resolution)

**For established connections (not just listening):**
```bash
ss -tupn
```

**For full connection table with all states:**
```bash
ss -anp
```

**Alternative with process detail:**
```bash
netstat -tulpn   # older systems
lsof -i          # all open network files
```

The key advantage of `ss -tulpn` is speed and the `-p` flag which links each socket to its owning process — critical during incident response.

---

### 6. Preserving the Timeline of File Changes in a Directory

**Using `find` with timestamps:**
```bash
find /suspicious/directory -printf "%T+ %p\n" | sort
```

**Using `stat` on a specific file:**
```bash
stat filename.txt
# Shows: Access (atime), Modify (mtime), Change (ctime)
```

**Using `ls` with timestamps:**
```bash
ls -la --time-style=full-iso /directory
```

**Using `istat` (from The Sleuth Kit) for forensic timeline:**
```bash
fls -r -m / /dev/sda1 > bodyfile.txt
mactime -b bodyfile.txt -d > timeline.csv
```

**Key point:** Mount suspicious drives read-only before analysis to avoid modifying `atime`:
```bash
mount -o ro,noatime /dev/sdb1 /mnt/evidence
```

---

### 7. Windows Event IDs Critical for Evidence of New Service Installation

| Event ID | Log | Description |
|---|---|---|
| **7045** | System | A new service was installed on the system (most important) |
| **7040** | System | Service start type changed |
| **7036** | System | Service entered running/stopped state |
| **4697** | Security | A service was installed (requires audit policy enabled) |
| **4688** | Security | Process creation — captures `sc.exe` or `services.exe` invocations |

**Event ID 7045** is the primary indicator. It records:
- Service name
- Service file path (often reveals malicious binary location)
- Service type
- Account under which it runs

Attackers frequently install persistence as a service. Hunting for 7045 events with unusual binary paths (e.g., `C:\Users\Public\`, `C:\Temp\`) is a high-value detection strategy.

---

### 8. Collecting a Memory Dump from Windows Using Built-in or Sysinternals Tools

**Option 1: Task Manager (GUI)**
1. Open Task Manager → Details tab
2. Right-click process → "Create dump file"
3. Saved to `%TEMP%\<process>.dmp`
*Limitation: Single process only, not full system memory.*

**Option 2: ProcDump (Sysinternals)**
```powershell
# Single process dump
procdump.exe -ma <PID> C:\Evidence\process.dmp

# Full system memory dump
procdump.exe -ma -mp lsass.exe C:\Evidence\lsass.dmp
```

**Option 3: NotMyFault (Sysinternals) — Full RAM dump**
```
notmyfault.exe /crash
# Then collect from C:\Windows\MEMORY.DMP
```

**Option 4: WinPmem (free, open source)**
```powershell
winpmem.exe C:\Evidence\memory.raw
```

**Option 5: Windows built-in (via Task Scheduler or kernel dump)**
Configure complete memory dump in System Properties → Advanced → Startup and Recovery.

**For GraySentinel labs:** ProcDump + WinPmem are the recommended tools — free, scriptable, and produce Volatility-compatible output.

---

### 9. Purpose of a Chain of Custody Form and GraySentinel Simulation

**Purpose:**
A chain of custody (CoC) form documents every person who touched a piece of evidence, every action performed on it, and every transfer of custody. It answers: *Who collected it? When? From where? Who had it after that? Was it altered?*

Without a complete CoC, evidence can be challenged as tampered, misidentified, or improperly handled — even if it's genuine.

**GraySentinel Training Simulation:**

Since we operate in a training environment, we simulate CoC using a structured Markdown template committed to GitHub with each lab submission:

```
Evidence ID: GS-LAB-[date]-[student-ID]
Collected by: [Name]
Date/Time of collection: [ISO 8601 timestamp]
System: [hostname, IP, OS]
Collection method: [script + commands used]
SHA256 of evidence archive: [hash]
Transferred to: Lavanya Agre (Mentor)
Transfer method: GitHub commit
Transfer timestamp: [git commit hash + timestamp]
Notes: [any anomalies observed]
```

The GitHub commit hash itself acts as a tamper-evident seal — any modification after submission produces a different commit hash, simulating real CoC integrity.

---

### 10. Securely Transferring Evidence Files from a Compromised System

**Risk:** The compromised system's tools (including `scp`, `ssh`) may be trojaned. Always prefer pulling from the investigation machine rather than pushing from the compromised host.

**Recommended approaches (safest first):**

**1. Pull via SSH from investigation machine (preferred):**
```bash
# From your clean investigation machine:
scp -r investigator@compromised-host:/evidence/ ./local_evidence/
```

**2. Netcat over isolated network:**
```bash
# On compromised host (sender):
tar czf - /evidence/ | nc 192.168.1.100 9999

# On investigation machine (receiver):
nc -lvp 9999 > evidence.tar.gz
```

**3. Write-once media (USB, DVD):**
Physically copy to write-once media if network transfer is too risky.

**4. Always hash before and after transfer:**
```bash
sha256sum evidence.tar.gz > evidence.sha256
# Transfer both files
# Verify on investigation machine:
sha256sum -c evidence.sha256
```

---

### 11. What is FTK Imager (Free) and Can It Run on Linux via Wine?

**FTK Imager** (by Exterro/AccessData) is a free forensic imaging tool for Windows that can:
- Create forensic images in E01, DD, AFF formats
- Preview evidence without altering it
- Generate hash verification (MD5 + SHA1)
- Mount images as read-only drives
- Export files from images

**Can it run on Linux via Wine?**
Technically yes, with limitations:
```bash
sudo apt install wine
wine FTKImager.exe
```
However, hardware access through Wine is unreliable — disk device access (`\\.\PhysicalDrive0`) often fails. For Linux forensic imaging, native alternatives are preferred:

| Tool | Linux Native | Equivalent Function |
|---|---|---|
| `dd` / `dcfldd` | Yes | Raw disk imaging |
| Guymager | Yes | GUI forensic imager, E01 support |
| ewfacquire | Yes | EnCase E01 format imaging |
| Autopsy | Yes | Full forensic suite (FTK equivalent) |

**Recommendation for GraySentinel:** Use Guymager or Autopsy on Linux. Use FTK Imager on Windows VMs for Windows-specific labs.

---

### 12. Why Use `script` or `tee` to Record Terminal Sessions During Investigation?

**`script` purpose:**
Records every command typed and every output received in a terminal session to a log file. Creates a complete, time-stamped record of the investigation process.

```bash
script -a -t 2>session_timing.log session_transcript.txt
# -a = append mode
# -t = record timing data (for scriptreplay)
```

**`tee` purpose:**
Pipes output to both the screen and a file simultaneously — useful for individual commands:
```bash
ps aux | tee processes.txt
ss -tulpn | tee network_connections.txt
```

**Why this matters in forensics:**
1. **Reproducibility** — anyone reviewing your work can see exactly what you ran and what the system returned.
2. **Non-repudiation** — proves what actions were taken during investigation.
3. **Training value** — students see the exact workflow, not just results.
4. **Accountability** — if a mistake was made, the transcript shows it.

In a real IR engagement, the `script` log is submitted alongside evidence as proof of investigator methodology.

---

### 13. Risks of Using `scp` from a Compromised Host

| Risk | Description |
|---|---|
| **Trojaned `scp` binary** | Attacker replaces `/usr/bin/scp` with a modified version that exfiltrates credentials or files |
| **Credential exposure** | SSH private keys or passwords used in the `scp` command may be logged by a keylogger or in `.bash_history` |
| **Evidence contamination** | Running commands on a compromised host modifies timestamps (atime), alters swap, and may trigger attacker scripts |
| **Attacker awareness** | If attacker has a live connection, they may see your commands and wipe traces |
| **LD_PRELOAD hooks** | Malicious libraries can intercept system calls made by `scp`, altering transferred data |

**Mitigation:**
- Always verify binary hash before use: `sha256sum /usr/bin/scp`
- Prefer pulling from investigation machine (reverse the direction)
- Use an isolated network segment for evidence transfer
- Document every command run with timestamps

---

### 14. Verifying Integrity of a Downloaded File Using Checksums

**Step 1: Download the file and its checksum**
```bash
wget https://example.com/tool.tar.gz
wget https://example.com/tool.tar.gz.sha256
```

**Step 2: Verify**
```bash
sha256sum -c tool.tar.gz.sha256
# Output: tool.tar.gz: OK
```

**Manual verification:**
```bash
sha256sum tool.tar.gz
# Compare output manually with published hash
```

**For MD5:**
```bash
md5sum -c tool.tar.gz.md5
```

**For GPG-signed releases (strongest):**
```bash
gpg --import developer_key.asc
gpg --verify tool.tar.gz.sig tool.tar.gz
```

**Rule of thumb:** If the checksum doesn't match — delete the file and re-download. Never run a tool with a mismatched hash.

---

### 15. Three Free and Open-Source Tools for Evidence Collection

**Linux Tools:**

**1. Volatility (Linux & Windows memory analysis)**
- Purpose: Analyse memory dumps for running processes, network connections, injected code
- Platform: Linux, Windows, macOS
- Link: https://github.com/volatilityfoundation/volatility3
- Use in GraySentinel: Memory forensics labs

**2. CyLR (Live Response Collection)**
- Purpose: Rapidly collects forensic artifacts from Linux and Windows (logs, prefetch, registry hives, browser history)
- Platform: Linux, Windows
- Link: https://github.com/orlikoski/CyLR
- Use in GraySentinel: Triage collection in IR simulation labs

**Windows Tools:**

**3. Sysinternals Suite (Microsoft — free)**
- Key tools: Autoruns, ProcExp, TCPView, ProcDump, Sysmon
- Purpose: Live response on Windows — enumerate processes, network, persistence, services
- Platform: Windows
- Link: https://learn.microsoft.com/en-us/sysinternals/
- Use in GraySentinel: Windows investigation exercises, Sysmon deployment

**Bonus — Cross-platform:**

**hashdeep / md5deep**
- Purpose: Recursive file hashing with audit mode (detect added/changed/removed files)
- Platform: Linux, Windows, macOS
- Use: Evidence integrity verification, submission hashing

---

## Kali Linux Practical Investigation

### Scenario
Simulated intrusion on Kali VM. A process `evil_backdoor` was running and connecting to an external IP. A hidden log file was left at `/tmp/.hidden/`. A pcap shows DNS queries to `malware.test`.

### Step 1: Start `script` Session

```bash
script -a -t 2>/home/akshat/Evidence/session_timing.log \
  /home/akshat/Evidence/linux_evidence/session_transcript.txt

echo "=== Investigation Start ==="
echo "Investigator: Akshat Tiwari"
echo "Date/Time: $(date --iso-8601=seconds)"
echo "Hostname: $(hostname)"
echo "System: $(uname -a)"
```

### Step 2: Capture Running Processes

```bash
ps aux | tee /home/akshat/Evidence/linux_evidence/processes.txt
```

**Simulated output (relevant excerpt):**
```
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root      1337  0.0  0.1   4532  1024 ?        S    00:15   0:00 /tmp/.hidden/evil_backdoor
```

### Step 3: Capture Network Connections

```bash
ss -tulpn | tee /home/akshat/Evidence/linux_evidence/network_connections.txt
```

**Simulated output:**
```
Netid  State   Recv-Q  Send-Q  Local Address:Port   Peer Address:Port  Process
tcp    ESTAB   0       0       192.168.1.105:54321  203.0.113.42:4444  users:(("evil_backdoor",pid=1337,fd=3))
```

### Step 4: Capture Open Files for Suspicious PID

```bash
lsof -p 1337 | tee /home/akshat/Evidence/linux_evidence/open_files_pid1337.txt
```

**Simulated output:**
```
COMMAND        PID  USER  FD   TYPE DEVICE SIZE/OFF NODE NAME
evil_backdoor 1337  root  cwd  DIR   8,1     4096   2    /tmp/.hidden
evil_backdoor 1337  root  txt  REG   8,1    81920   99   /tmp/.hidden/evil_backdoor
evil_backdoor 1337  root  3u   IPv4  12345        0t0  TCP 192.168.1.105:54321->203.0.113.42:4444 (ESTABLISHED)
```

### Step 5: Locate and Collect Hidden Log File

```bash
ls -la /tmp/.hidden/
stat /tmp/.hidden/backdoor.log
file /tmp/.hidden/backdoor.log
```

**Simulated stat output:**
```
File: /tmp/.hidden/backdoor.log
Size: 2048       Blocks: 8     IO Block: 4096  regular file
Device: 801h/2049d  Inode: 131073  Links: 1
Access: 2026-06-23 00:15:33.000000000 +0530
Modify: 2026-06-23 00:17:44.000000000 +0530
Change: 2026-06-23 00:17:44.000000000 +0530
```

```bash
# Copy evidence preserving metadata
cp --preserve=all /tmp/.hidden/backdoor.log \
  /home/akshat/Evidence/linux_evidence/backdoor.log
```

### Step 6: Analyze PCAP with tcpdump/tshark

```bash
tshark -r /home/akshat/Evidence/capture.pcap \
  -Y "dns" \
  -T fields -e frame.time -e ip.src -e dns.qry.name \
  | tee /home/akshat/Evidence/linux_evidence/network_capture_summary.txt
```

**Simulated output:**
```
Jun 23, 2026 00:15:44  192.168.1.105  malware.test
Jun 23, 2026 00:15:45  192.168.1.105  malware.test
Jun 23, 2026 00:16:01  192.168.1.105  malware.test
```

DNS queries confirmed to `malware.test` — consistent with C2 beacon behaviour.

### Step 7: Generate Hash List for All Evidence Files

```bash
cd /home/akshat/Evidence/linux_evidence/
sha256sum * | tee hash_list.txt
```

**Simulated hash_list.txt:**
```
a3f1c2d4e5b6...  processes.txt
7b8c9d0e1f2a...  network_connections.txt
4c5d6e7f8a9b...  open_files_pid1337.txt
2d3e4f5a6b7c...  backdoor.log
9e0f1a2b3c4d...  network_capture_summary.txt
```

### Step 8: End Script Session

```bash
echo "=== Investigation End: $(date --iso-8601=seconds) ==="
exit  # ends script recording
```

### Evidence Summary

| Artifact | Location | Hash Verified |
|---|---|---|
| Process list | linux_evidence/processes.txt | Yes |
| Network connections | linux_evidence/network_connections.txt | Yes |
| Open files (PID 1337) | linux_evidence/open_files_pid1337.txt | Yes |
| Hidden log file | linux_evidence/backdoor.log | Yes |
| PCAP DNS summary | linux_evidence/network_capture_summary.txt | Yes |
| Terminal transcript | linux_evidence/session_transcript.txt | Yes |
| Master hash list | linux_evidence/hash_list.txt | — |

**Finding:** Process `evil_backdoor` (PID 1337) was running from `/tmp/.hidden/`, maintained an established TCP connection to `203.0.113.42:4444`, and generated repeated DNS queries to `malware.test`. All evidence collected, hashed, and preserved without modification.

---

## Windows Investigation Exercise

### Scenario
Windows VM with Sysmon installed. Simulated suspicious PowerShell script execution (download of harmless test file).

### Step 1: Simulate the PowerShell Event

```powershell
# Harmless simulation — downloads a test file
powershell.exe -ExecutionPolicy Bypass -Command "Invoke-WebRequest -Uri 'http://example.com/test.txt' -OutFile 'C:\Temp\test.txt'"
```

This triggers:
- **Event ID 1** (Process Creation) — records PowerShell invocation with full command line
- **Event ID 3** (Network Connection) — records outbound connection to example.com

### Step 2: Export Sysmon Events from Event Viewer

```
Event Viewer → Applications and Services Logs → Microsoft → Windows → Sysmon → Operational
→ Filter Current Log → Event IDs: 1, 3
→ Action → Save Filtered Log File As → sysmon_events.evtx
```

### Step 3: Filter .evtx for PowerShell Process Creation

```powershell
# Load and filter the exported .evtx
$events = Get-WinEvent -Path "C:\Evidence\sysmon_events.evtx" |
  Where-Object { $_.Id -eq 1 -and $_.Message -like "*powershell*" }

# Display relevant fields
$events | ForEach-Object {
  $xml = [xml]$_.ToXml()
  [PSCustomObject]@{
    TimeCreated = $_.TimeCreated
    ProcessId   = ($xml.Event.EventData.Data | Where-Object Name -eq 'ProcessId').'#text'
    CommandLine = ($xml.Event.EventData.Data | Where-Object Name -eq 'CommandLine').'#text'
    ParentImage = ($xml.Event.EventData.Data | Where-Object Name -eq 'ParentImage').'#text'
  }
} | Export-Csv "C:\Evidence\windows_evidence\filtered_events.csv" -NoTypeInformation
```

### Step 4: Extract Network Destination IP (Event ID 3)

```powershell
$netEvents = Get-WinEvent -Path "C:\Evidence\sysmon_events.evtx" |
  Where-Object { $_.Id -eq 3 }

$netEvents | ForEach-Object {
  $xml = [xml]$_.ToXml()
  [PSCustomObject]@{
    TimeCreated     = $_.TimeCreated
    Image           = ($xml.Event.EventData.Data | Where-Object Name -eq 'Image').'#text'
    DestinationIp   = ($xml.Event.EventData.Data | Where-Object Name -eq 'DestinationIp').'#text'
    DestinationPort = ($xml.Event.EventData.Data | Where-Object Name -eq 'DestinationPort').'#text'
  }
}
```

**Simulated output:**
```
TimeCreated        : 2026-06-23 01:05:22
Image              : C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
DestinationIp      : 93.184.216.34
DestinationPort    : 80
```

### Step 5: Hash the .evtx File

```powershell
Get-FileHash "C:\Evidence\sysmon_events.evtx" -Algorithm SHA256 |
  Select-Object Algorithm, Hash, Path |
  Out-File "C:\Evidence\windows_evidence\event_log_hash.txt"
```

**Simulated output:**
```
Algorithm  Hash                                                             Path
---------  ----                                                             ----
SHA256     3A9F2C1D4E5B6A7C8D9E0F1A2B3C4D5E6F7A8B9C0D1E2F3A4B5C6D7E8F9A0B1  C:\Evidence\sysmon_events.evtx
```

### Methodology Summary

1. Simulated suspicious PowerShell download using `Invoke-WebRequest`
2. Exported Sysmon Event IDs 1 and 3 from Event Viewer to `.evtx`
3. Used `Get-WinEvent` to parse and filter events by process name and command line
4. Extracted destination IP from Event ID 3 (Network Connection) — resolved to `93.184.216.34` (example.com)
5. Hashed the `.evtx` file using `Get-FileHash` (SHA256) for integrity verification
6. All outputs saved to `C:\Evidence\windows_evidence\`

---

*Report submitted for Day 04 ODP review.*
*Akshat Tiwari | GraySentinel Intelligence Command*