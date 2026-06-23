# How to Collect Evidence Like a Pro
*A GraySentinel Guide for Students Starting Their First Labs*
**Author:** Akshat Tiwari | Intelligence Command

---

You just got access to a lab. Something suspicious happened. Now what?

Most beginners make the same mistake: they start clicking around, running random commands, taking screenshots, and then wonder why their report looks like a mess. Here is how professionals do it — and how you should too.

---

## Step 1: Record Everything Before You Touch Anything

The first command you run in any investigation is this:

```bash
script -a /home/kali/Evidence/session_transcript.txt
```

This starts recording your entire terminal session — every command you type, every output you get. When you are done, type `exit` to stop.

**Why?** Because if you cannot show your work, your findings mean nothing. A screenshot of a result with no record of how you got there is not evidence — it is a claim.

Also note the system time immediately:

```bash
date --iso-8601=seconds
timedatectl
```

Time discrepancies between the suspect system and your machine matter in real investigations.

---

## Step 2: Collect in the Right Order (Most Volatile First)

RAM and running processes disappear the moment you reboot. Disk files stay. So always collect in this order:

**1. Running processes:**
```bash
ps aux | tee processes.txt
```

**2. Network connections (with process names):**
```bash
ss -tulpn | tee network_connections.txt
```

**3. Open files for a suspicious PID:**
```bash
lsof -p <PID> | tee open_files.txt
```

**4. Then disk artifacts** — log files, hidden directories, config files.

Never reboot before completing steps 1–3. You will lose the most important data.

---

## Step 3: Hash Everything You Collect

After collecting each file, generate a SHA256 hash:

```bash
sha256sum backdoor.log >> hash_list.txt
```

At the end, hash your entire evidence folder:

```bash
sha256sum Evidence/* | tee master_hash_list.txt
```

**Why?** The hash proves the file was not modified after you collected it. If the hash at collection matches the hash at submission — the evidence is intact. No hash = no proof of integrity.

---

## Step 4: Organise Your Evidence Folder

Do not dump everything in one folder. Structure it:

```
Evidence/
  linux_evidence/
    processes.txt
    network_connections.txt
    open_files_pid1337.txt
    backdoor.log
    session_transcript.txt
    hash_list.txt
```

Label files clearly. `output1.txt` tells nobody anything. `network_connections_2026-06-23.txt` tells everyone exactly what it is.

---

## Step 5: Write What You Found, Not What You Think

An evidence report is not a story. It is a record of facts:

- What artifact was found
- Where it was located
- When it was created/modified (use `stat`)
- What its SHA256 hash is

Leave the analysis and conclusions for the separate analysis section. Evidence presentation and interpretation are two different documents.

---

## Quick Checklist Before Submitting

- [ ] `script` session transcript included
- [ ] System time and timezone noted
- [ ] `ps aux` output saved
- [ ] `ss -tulpn` output saved
- [ ] All files hashed with SHA256
- [ ] Evidence folder structured correctly
- [ ] No files modified after collection (hashes match)

If you cannot check every box — go back and fix it before submitting.

---

*GraySentinel Intelligence Command | ODP Training Material*