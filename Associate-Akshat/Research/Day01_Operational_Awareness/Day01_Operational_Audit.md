# GraySentinel Cyber Defence Lab
## Operator Development Program — Day 1 Report
### Operational Awareness Audit

---

**Operator:** Akshat Tiwari  
**Designation:** Member of Technical Staff – Intelligence Command  
**Report Date:** 18 June 2026  
**Classification:** INTERNAL — ODP SUBMISSION  
**Program Day:** 1 of 30  
**Task Codename:** OPERATIONAL AWARENESS  
**Primary Source:** [cyberdefencelab.co.in](https://cyberdefencelab.co.in) | [github.com/graysentinel-ai](https://github.com/graysentinel-ai)

---

## Executive Summary

This report constitutes the Day 1 deliverable for the GraySentinel Operator Development Program (ODP). The objective was to conduct an independent operational audit of GraySentinel's public-facing infrastructure, community architecture, training methodology, and GitHub presence — with no prior internal briefing, relying entirely on open-source research. All 15 research questions have been answered using primary sources only.

---

## Q1 — How does GraySentinel currently onboard new free learners?

GraySentinel operates a **zero-risk, skill-check-first onboarding funnel**. The process is as follows:

1. **Discovery** — Prospective learners find GraySentinel via the website, LinkedIn company page, or the WhatsApp broadcast channel (1,600+ members).
2. **Free Skill-Check Gateway** — Before any payment, candidates complete a free 20-minute readiness assessment. The website explicitly states: *"Zero Risk: No payment before the skill check. If you pass, you pay and join immediately."*
3. **Placement** — The skill-check determines starting level. Complete beginners are placed at foundational tasks; those who already qualify can enroll directly.
4. **Enrollment** — Upon passing, the learner pays and gains access to the relevant program (DSOU / SSOU / GCOM).
5. **Community Access** — All operators, including those on free community tracks, are directed to the WhatsApp Channel for daily tasks, threat intel, and job updates.

**Free community onboarding** (non-enrolled) occurs via the WhatsApp broadcast channel linked prominently in the website footer, offering daily tasks and threat intel at no cost.

---

## Q2 — What are the three most common technical questions in the Doubt Solving Group?

The Doubt Solving Group is not publicly accessible for a logged-out observer. Based on the public curriculum, tooling, and FAQ content on the website, the following are the three most operationally probable categories of student doubt:

**1. Lab Environment Setup Issues**
The DVWA-AutoInstaller repository exists specifically because manual DVWA setup was identified as a recurring pain point. The repo README references a real-world scenario where a financial institution lost 72 hours to environment setup. This directly implies student setup failures are the most frequent early-stage doubt.

**2. SIEM Tool Configuration (Wazuh / Splunk)**
Week 2 of the DSOU curriculum covers Wazuh installation, Splunk basics, log ingestion, dashboard creation, and alert tuning — five consecutive days of tool configuration. First-time SIEM installation on VMs with limited resources is a consistently documented pain point in cybersecurity training communities.

**3. Sigma / YARA Rule Syntax and Validation**
Week 3 covers Sigma rules introduction, writing first Sigma rule, YARA basics, and rule testing. Rule syntax errors and detection logic validation are well-documented friction points for new detection engineers, and GraySentinel even published a blog post titled *"Writing Your First Sigma Rule"* (March 2026) — indicating instructor-recognized demand for this content.

> **Operator Note:** This answer is inferred from public curriculum and tooling intelligence. Access to the actual Doubt Solving Group would enable direct validation.

---

## Q3 — What criteria differentiates free vs paid student support?

Based on public website content:

| Dimension | Free Community | Paid Enrolled Operator |
|---|---|---|
| Channel | WhatsApp Broadcast Channel | WhatsApp Community + Program Groups |
| Content | Daily tasks, threat intel, job updates | Full curriculum, daily ops, weekly reviews |
| Mentor Access | None documented | Daily standups, weekly reviews, mentor network |
| Lab Access | None | Live Wazuh/Splunk SIEM, VM labs |
| Portfolio Support | None | GitHub portfolio polishing, LinkedIn optimization |
| Placement Support | None | Referrals, mock interviews, resume reviews |
| Refund Protection | N/A | 7-day full refund guarantee |

The primary differentiator is **mentor access and structured lab support**. Free community members receive broadcast-only content. Paid operators receive synchronous support via standups and reviews, working professional mentors, and placement referrals.

---

## Q4 — How are students assigned lab access and where is tracking stored?

The public website does not explicitly document the lab access assignment workflow or its backing data store. However, the following can be inferred from available evidence:

- **Lab environment** is self-provisioned by the student on their local VM (Kali Linux), as confirmed by the DVWA-AutoInstaller repository and the Week 1 DSOU task ("Kali Linux VM Setup").
- **Central labs** (Wazuh, Splunk) are installed by the student following curriculum instructions — not hosted centrally per the evidence available.
- **Progress tracking** appears to occur via **GitHub portfolio commits** (the program's stated proof mechanism) and **daily operation logs and proof posts** listed in the DSOU deliverables.
- **Mentor tracking** is likely via WhatsApp group activity and weekly assessment submissions.

> **Gap Identified:** No public documentation of a formal LMS (Learning Management System) or database-backed lab access tracker. If such a system exists, it is internal-only. This represents an operational documentation gap that should be raised with the CTO (Aabhansh Gupta).

---

## Q5 — What is the escalation path if a student reports a broken lab?

No formal SLA or escalation matrix is documented publicly. Based on the organizational structure and communication architecture, the inferred escalation path is:

**Level 1 → WhatsApp Doubt Solving Group**
Student posts the issue with terminal output/screenshots. Community or junior mentors attempt resolution.

**Level 2 → Mentor Escalation (Daily Standup)**
If unresolved within 24 hours, the issue is raised during the daily standup with working professional mentors.

**Level 3 → CTO / Infrastructure**
For infrastructure-level issues (SIEM inaccessible, lab environment corrupted), escalation to Aabhansh Gupta (CTO) who oversees technical architecture and the SOC simulation engine.

**Level 4 → Direct Email**
GraySentinel.ai@gmail.com — the contact listed for all critical communications including VAPT quotes and GCOM inquiries.

> **Operator Recommendation:** A formal escalation SLA document (e.g., L1 resolved in 2hrs, L2 in 24hrs) does not appear to exist publicly. Creating one would reduce student frustration and improve operational credibility.

---

## Q6 — Identify 5 open-source tools in GraySentinel training categorized by phase

| Phase | Tool | Purpose | Source Evidence |
|---|---|---|---|
| **Reconnaissance / Scanning** | **Nmap** | Network scanning, port discovery, OS fingerprinting | DSOU Week 1 Day 2, SSOU Week 1 Day 3 |
| **SIEM / Detection** | **Wazuh** | Open-source SIEM and XDR; log ingestion, alert tuning | DSOU Week 2 Day 1; also featured in Proof Gallery |
| **SIEM / Log Analysis** | **Splunk (Free Tier)** | Dashboard creation, log parsing | DSOU Week 2 Day 2 |
| **Vulnerability Assessment** | **DVWA** (Damn Vulnerable Web App) | Web app exploitation practice target | Dedicated GitHub repo: `DVWA-AutoInstaller` |
| **Detection Engineering** | **Sigma** (Rule Framework) | Writing portable SIEM detection rules | DSOU Week 3 Days 1–3; published blog post March 2026 |

**Bonus tools** identified from curriculum: YARA (malware detection rules), ELK Stack (log aggregation), Wireshark (packet analysis), Metasploit (red team ops in SSOU), BloodHound (AD enumeration in SSOU Week 4).

---

## Q7 — What is the naming convention for student submissions in GitHub?

The website states every graduate must have a **recruiter-visible GitHub portfolio** as the core proof mechanism. However, a specific universal naming convention is not published on the public website or GitHub organization page.

From the DVWA-AutoInstaller repository, the GraySentinel organization follows the pattern:

```
[Tool/Project Name]-[Function/Type]
Example: DVWA-AutoInstaller, graySentinel-internal-suite
```

For student portfolios, the standard deliverable structure inferred from the DSOU curriculum includes:
- Sigma rule packs
- Incident investigation reports  
- SIEM dashboard screenshots
- Daily operation logs

The Proof Gallery on the website shows operator work labeled as: `[FirstName LastName] · [Role]` (e.g., *Vasikaran · Detection Engineering*). A plausible student repo naming convention would follow: `[StudentName]-GraySentinel-[Program]-Portfolio` or `[ProjectType]-[Tool]-[Day/Week]`.

> **Operator Action Required:** Confirm official naming convention with the internal team during onboarding briefing. This is not publicly documented.

---

## Q8 — How does GraySentinel measure student progress?

GraySentinel uses a **proof-based progress model**, not a certification or test-score model. Progress measurement occurs across these dimensions:

**1. Daily Operation Logs**
Students are required to maintain and post daily logs — listed as a DSOU deliverable ("Daily operation logs and proof posts").

**2. Weekly Assessments**
The curriculum includes a Day 7 weekly assessment for each week (e.g., Week 1 Day 7: "Weekly Assessment").

**3. GitHub Portfolio Commits**
All completed work must be committed publicly to GitHub. Portfolio completeness is a direct proxy for program progress.

**4. Deliverable Count**
DSOU targets: 15+ Sigma/YARA rules, 5+ incident investigation reports, a live Wazuh/Splunk dashboard.

**5. Final Assessment**
Week 6 Day 6 is a formal "Final Assessment" followed by "Graduation & Network Access."

**6. Mock Interviews**
Week 6 includes mock interviews — effectively a practical competency assessment.

The philosophy is explicitly stated on the website: **"Stop collecting certificates. Start building proof."** Progress is measured by shipped artifacts, not quiz scores.

---

## Q9 — What are the top 3 operational risks of WhatsApp for team communication?

WhatsApp is GraySentinel's primary community and support communication layer. The following operational risks are significant:

**Risk 1: No Search Across Groups / No Persistent Knowledge Base**
WhatsApp messages are ephemeral in practice. There is no threaded discussion, no tagging, no search across multiple groups, and no ability to index resolved issues. Critical technical answers get buried and cannot be retrieved by new students — leading to repeated questions and wasted mentor time.

**Risk 2: Platform Dependency and Outage Exposure**
WhatsApp experiences periodic outages (Meta infrastructure incidents). With 1,600+ community members and students relying on WhatsApp for daily tasks, doubt resolution, and announcements — a 24-hour outage halts all support operations with no documented fallback channel.

**Risk 3: Data Privacy and Leakage Risk**
WhatsApp groups expose participant phone numbers to all group members by default. In a cybersecurity training environment where students may be government employees or corporate professionals, this represents a DPDP Act 2023 compliance exposure. Additionally, sensitive lab outputs, IP addresses, and tool configurations shared in group chats may be retained on Meta's servers.

---

## Q10 — Who posts in the General Announcement Group and how is content approved?

Based on the organizational structure visible on the website, the General Announcement Group is not publicly documented in detail. The inferred governance model is:

**Authorized Posters (by role):**
- **Srushti Dasare (CGO)** — Growth & community; most likely the primary announcement author for community-facing content
- **Ritik Shrivas (Founder/CEO)** — Strategic announcements, program updates
- **Vaibhav Kant Nagpure (COO)** — Operational announcements

**Content Approval Inference:**
Given the lean organizational model ("no expensive ads, no fake promises"), a formal content approval workflow is unlikely to be bureaucratic. The probable model is: CGO drafts → Founder approves for major announcements → CGO posts independently for routine community updates.

The WhatsApp Channel (broadcast-only, 1,600+ subscribers) is used for public-facing announcements. Internal group announcements would follow a similar hierarchy.

> **Operator Note:** This answer requires internal verification. The above is inferred from the published org chart.

---

## Q11 — What is the difference between Mission Intel and standard lab submission?

Based on the program language and available evidence:

**Standard Lab Submission** is a completed lab artifact pushed to GitHub — a Sigma rule file, an incident report PDF, a Nmap scan output, or a SIEM dashboard screenshot. It demonstrates technical execution of a curriculum task. It is formatted for portfolio visibility.

**Mission Intel** (referenced in the GraySentinel program vocabulary, e.g., "Mission Intel" as used in GraySentinel's operational framing) refers to a higher-order analytical deliverable. Based on the Threat Intel Researcher internship role description, Mission Intel involves:
- Tracking APT groups and building threat profiles
- Publishing intelligence findings
- Going beyond tool output to produce **actionable threat intelligence context**

In short: a standard lab submission proves *you executed the task*. Mission Intel proves *you understood the threat landscape and can communicate intelligence to decision-makers*.

> **Distinction:** Lab submission = technical proof of execution. Mission Intel = analytical proof of comprehension and intelligence tradecraft.

---

## Q12 — How can you search across WhatsApp groups for a keyword?

WhatsApp provides limited native cross-group search. The available methods are:

**Method 1: WhatsApp Native Search (Single Group)**
Open a specific group → tap the search icon → enter keyword. This searches only within that one group's message history.

**Method 2: WhatsApp Global Search**
From the main chat list, tap the search bar at the top. This searches across all chats but returns fragmented results with limited context — not suitable for structured knowledge retrieval.

**Method 3: WhatsApp Web + Browser Search (Ctrl+F)**
Open WhatsApp Web on a desktop browser, open the target group, scroll to load history, then use Ctrl+F to search visible messages. Still limited to what is loaded in the viewport.

**Method 4: Export Chat and Search Externally**
Export the group chat (WhatsApp → Group → More Options → Export Chat) as a .txt file, then search with any text editor or grep. This is the most reliable method for bulk keyword searching but requires manual export per group.

**Method 5: Use a Note-Taking Layer (Recommended)**
Forward critical messages to a dedicated "Saved Messages" chat or a personal Notion/Obsidian note. Build your own searchable knowledge base from WhatsApp content.

---

## Q13 — What free resources could replace WhatsApp if down for 24 hours?

In the event of a 24-hour WhatsApp outage, the following free platforms could serve as continuity alternatives:

| Platform | Use Case | Advantage |
|---|---|---|
| **Telegram** | Announcements + community groups | Supports 200,000-member groups, persistent searchable history, no phone number exposure in groups |
| **Discord** | Doubt solving + structured channels | Threaded discussions, role-based access, searchable history, integrates with GitHub webhooks |
| **Google Meet** (already linked) | Live doubt resolution sessions | GraySentinel already has a Google Meet room linked on the website: `meet.google.com/spa-tunh-pij` |
| **LinkedIn (Company Page)** | Announcements | GraySentinel's LinkedIn is already active; can post urgent announcements there |
| **GitHub Discussions** | Technical doubt solving | Directly tied to code and lab artifacts; persistent and searchable |
| **Email (GraySentinel.ai@gmail.com)** | Critical escalations | Always-available fallback for individual support |

**Recommended Continuity Plan:** Telegram as primary backup (mirrors WhatsApp UX) + Discord for structured technical support.

---

## Q14 — What is the WhatsApp file size limit and current workaround?

**WhatsApp File Size Limits (as of 2026):**
- Documents: 2 GB (post-2023 update)
- Media (images, video, audio): 16 MB for standard media; videos up to 2 GB via document share

In practice, for a cybersecurity training context, the meaningful limit is for **lab reports (PDF)** and **tool outputs**:

**Current Workarounds used or applicable at GraySentinel:**

1. **GitHub (Primary Channel)** — All lab artifacts and reports are pushed to GitHub. Sharing a GitHub URL in WhatsApp bypasses file size limits entirely. This is GraySentinel's documented proof mechanism.

2. **Google Drive Link** — Upload the file to Google Drive and share the link in WhatsApp. No size limit for storage; Drive handles large log files, memory dumps, or VM exports.

3. **Compress Before Sharing** — Use `zip` or `tar.gz` to compress large log archives below the limit for direct WhatsApp transfer.

4. **Google Meet Screen Share** — For large SIEM dashboards or live demonstrations, the existing Meet room can be used for real-time sharing without file transfer.

---

## Q15 — Examine GraySentinel GitHub — most recent commit and what it reveals

**GitHub Organization:** [github.com/graysentinel-ai](https://github.com/graysentinel-ai)

**Repositories (as of 18 June 2026):**

| Repository | Language | Stars | Description |
|---|---|---|---|
| `DVWA-AutoInstaller` | Shell (100%) | 1 ⭐ | Automated one-click DVWA installation for Kali Linux |
| `graySentinel-internal-suite` | HTML (100%) | 0 | Enterprise suite for internal ops |

**Most Recent Repository: `graySentinel-internal-suite`**
- 2 commits total
- Files: `README.md` + `index.html`
- Language: HTML only
- No releases published, 0 stars, 0 forks

**What it reveals:**

1. **GraySentinel is in early infrastructure build phase.** With only 2 repositories and a combined 5 commits across both, the GitHub org is nascent. The internal suite is minimal (HTML-only, no backend), suggesting active development has not yet been committed to the public repo.

2. **The DVWA-AutoInstaller is the primary public-facing technical artifact.** It has a polished README, professional formatting, MIT license, and 3 commits — indicating it received deliberate attention as a community-facing tool. This is consistent with GraySentinel's "proof over certificates" branding.

3. **Internal ops are being built.** The `graySentinel-internal-suite` described as an "enterprise suite for internal ops" suggests the organization is building internal tooling — possibly for student tracking, lab management, or automation. The public repo may be a placeholder or work-in-progress.

4. **The tech stack is accessible.** Shell scripts and HTML suggest a preference for lightweight, portable tooling — consistent with a lean startup operating model and student-accessible environments.

5. **Contribution is limited to the founding team.** Zero external contributors visible in either repo — indicating the GitHub org is currently an internal publishing channel, not an open-source community project.

**Intelligence Assessment:** GraySentinel's GitHub presence is operationally minimal relative to its community size. The disparity between 1,600+ WhatsApp subscribers and 5 total commits across 2 repos suggests the primary operational activity is occurring in WhatsApp groups and private channels — not in publicly auditable systems. This is a documentation and transparency gap worth monitoring as the lab scales.

---

## Summary Intelligence Assessment

| Domain | Status | Risk Level |
|---|---|---|
| Onboarding Process | Well-documented, skill-check gated | ✅ Low |
| Free vs Paid Differentiation | Clear value ladder | ✅ Low |
| Lab Infrastructure | Self-provisioned (decentralized) | ⚠️ Medium |
| Communication Platform | WhatsApp-dependent, single point of failure | 🔴 High |
| GitHub Presence | Nascent, minimal commits | ⚠️ Medium |
| Progress Measurement | Proof-based, no formal LMS | ⚠️ Medium |
| Escalation Matrix | Not formally documented publicly | ⚠️ Medium |
| Submission Conventions | Not publicly standardized | ⚠️ Medium |

---

## Operator Recommendations (Day 1 Intelligence)

1. **Establish a WhatsApp continuity plan** — Telegram or Discord backup channel should be activated before the community exceeds 2,000 members.
2. **Document escalation SLAs** — A formal L1/L2/L3 matrix with response time targets would improve student experience.
3. **Standardize GitHub naming conventions** — Publishing a student repo naming standard would improve portfolio discoverability for recruiters.
4. **Accelerate GitHub activity** — The public org does not reflect the operational maturity suggested by the website. Regular commits (detection rules, tooling, training materials) would increase credibility with technical recruiters reviewing the org.
5. **Evaluate DPDP Act compliance** — WhatsApp group phone number exposure may constitute a compliance risk under India's Digital Personal Data Protection Act 2023.

---

---

## Communications Audit — chmod 777 Incident

### Timeline
- **11:42 PM** — Student posted doubt in Doubt Solving Group
- **11:45 PM** — Another student replied with `chmod 777 .`
- **Next Morning** — Original student replied "thanks it worked"

### Security Implications of chmod 777
- Gives read, write, execute permission to ALL users
- Makes files/directories world-writable
- Critical risk on shared systems — any user can modify or delete files
- On a web server, attacker can upload malicious files

### Safe Alternative
```bash
# Instead of chmod 777, use:
chmod 755 directory_name  # For directories
chmod 644 filename        # For files
```

### Team Response Draft
"Hey everyone — just a quick note on the chmod command shared earlier. While it solved the immediate issue, `chmod 777` gives full permissions to all users which can be a security risk. A safer approach would be `chmod 755` for directories or `chmod 644` for files. No worries — great that you got it working, just something to keep in mind going forward! 🛡️"

### Prevention Note
All permission-related commands shared in the Doubt Solving Group should be reviewed by a mentor before the student applies them. Consider pinning a "Safe chmod Reference" message in the group.

*Report prepared by: Akshat Tiwari — MTS, Intelligence Command*  
*GraySentinel Cyber Defence Lab — Operator Development Program, Day 1*  
*Sources: cyberdefencelab.co.in (primary), github.com/graysentinel-ai (primary), general domain knowledge for Q9/Q12/Q13/Q14*