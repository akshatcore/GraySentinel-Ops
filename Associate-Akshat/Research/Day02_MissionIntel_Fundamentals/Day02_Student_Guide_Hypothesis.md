# How to Build a Strong Mission Intel Hypothesis

A hypothesis is your best explanation for *what happened*, stated so it can be proven or disproven with evidence — not a vague feeling that "something looks suspicious."

**A strong hypothesis is:**
- **Specific** — names the actor/process/asset involved.
- **Testable** — points to exact log sources or artifacts that would confirm or refute it.
- **Bounded by confidence** — states how sure you are and why.

**Weak hypothesis:**
> "There might be malware on the system because something weird happened in the logs."

This gives a reviewer nothing to validate — no process, no timeframe, no source.

**Strong hypothesis:**
> "An HTA file delivered via phishing spawned `powershell.exe` as a child process at 14:32 (Sysmon EID 1), consistent with initial-access-stage malware. Confidence: Medium — confirmed via process lineage, not yet cross-validated against network telemetry."

Build every Mission Intel hypothesis this way: claim → evidence source → confidence rating. If you can't name the log that would prove it, it's not a hypothesis yet — it's a guess.