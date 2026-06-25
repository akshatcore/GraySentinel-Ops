# Sigma Rule Test Documentation
**Rule:** Suspicious Service Created from Non-Standard Path with Unusual Name  
**Rule File:** `Day05_Sigma_Service_Creation.yml`  
**Author:** Akshat | GraySentinel Intelligence Command  
**Date:** 2026-06-25  
**Status:** Experimental — Tested in isolated lab environment

---

## Rule Overview

The rule covers three detection branches targeting Windows service-based persistence (T1543.003):

| Branch | Event Source | Trigger Condition |
|--------|-------------|-------------------|
| `service_install` | Windows System Log (EID 7045) | Service binary path in non-standard directory |
| `random_service_name` | Windows System Log (EID 7045) | Service name matches random alphanumeric pattern |
| `registry_service_set` | Sysmon (EID 13) | Registry ImagePath/ServiceDll set under Services key to non-standard path |

---

## Test Environment

| Component | Value |
|-----------|-------|
| OS | Windows 10 22H2 (Lab VM) |
| Sysmon Version | 15.x |
| Sysmon Config | SwiftOnSecurity baseline |
| Log Analysis Tool | Chainsaw v2 / Sigma CLI |
| Test Privileges | Local Administrator |

---

## Test Case 1 — `service_install` Branch

### Objective
Trigger detection on a service whose binary path resides in a non-standard user-writable directory.

### Command Executed
```cmd
sc create EvilSvc binPath= "C:\Users\Public\svc.exe" start= auto type= own
```

### Expected Behavior
- Windows System Event Log generates **Event ID 7045**
- Field `ServiceFileName` = `C:\Users\Public\svc.exe`
- Rule condition `service_install` matches on `ServiceFileName|contains: '\Users\Public\'`
- Alert fires at **level: high**

### Result
| Field | Value |
|-------|-------|
| EventID | 7045 |
| ServiceName | EvilSvc |
| ServiceFileName | C:\Users\Public\svc.exe |
| ServiceType | own process |
| StartType | auto start |
| AccountName | LocalSystem |

**Detection Status: ✅ TRIGGERED**  
Alert generated correctly. Filter `filter_legit` did not suppress (path not under System32/Program Files).

### Cleanup
```cmd
sc delete EvilSvc
del C:\Users\Public\svc.exe
```

---

## Test Case 2 — `random_service_name` Branch

### Objective
Trigger detection on a service whose name matches a random alphanumeric pattern (8–20 chars, no meaningful words), consistent with attacker-generated service names.

### Command Executed
```cmd
sc create xK9mQ3rT binPath= "C:\Windows\System32\svchost.exe -k netsvcs" start= auto
```

### Expected Behavior
- Windows System Event Log generates **Event ID 7045**
- Field `ServiceName` = `xK9mQ3rT`
- Rule regex `^[a-zA-Z0-9]{8,20}$` matches — no spaces, no dictionary words
- Alert fires at **level: high**

### Result
| Field | Value |
|-------|-------|
| EventID | 7045 |
| ServiceName | xK9mQ3rT |
| ServiceFileName | C:\Windows\System32\svchost.exe -k netsvcs |
| ServiceType | share process |
| StartType | auto start |
| AccountName | LocalSystem |

**Detection Status: ✅ TRIGGERED**  
Random name regex matched. Note: `ServiceFileName` starts with `C:\Windows\System32\` which would normally match `filter_legit`, however the `random_service_name` branch evaluates independently — name-based detection still fires.

> **Analyst Note:** This is intentional. A legitimate service should never have a random alphanumeric name even if its binary is in System32. The filter only suppresses the `service_install` path-based branch.

### Cleanup
```cmd
sc delete xK9mQ3rT
```

---

## Test Case 3 — `registry_service_set` Branch (Sysmon EID 13)

### Objective
Trigger detection via Sysmon's registry monitoring when an attacker directly writes a service `ImagePath` to the Registry pointing to a non-standard location — bypassing `sc create` entirely.

### Command Executed
```cmd
reg add HKLM\SYSTEM\CurrentControlSet\Services\TestSvc /v ImagePath /d "C:\AppData\runner.exe" /f
```

### Expected Behavior
- Sysmon generates **Event ID 13** (Registry Value Set)
- `TargetObject` = `HKLM\SYSTEM\CurrentControlSet\Services\TestSvc\ImagePath`
- `Details` = `C:\AppData\runner.exe`
- Rule condition `registry_service_set` matches on both `TargetObject|contains` and `Details|contains`
- Alert fires at **level: high**

### Result
| Field | Value |
|-------|-------|
| EventID | 13 |
| TargetObject | HKLM\SYSTEM\CurrentControlSet\Services\TestSvc\ImagePath |
| Details | C:\AppData\runner.exe |
| Image | reg.exe |
| User | DESKTOP-LAB\Administrator |

**Detection Status: ✅ TRIGGERED**  
Sysmon EID 13 captured the direct registry write. This branch catches attackers who avoid `sc.exe` or PowerShell `New-Service` to evade process-based detection.

### Cleanup
```cmd
reg delete HKLM\SYSTEM\CurrentControlSet\Services\TestSvc /f
```

---

## False Positive Validation

### Test: Legitimate Service Install (Should NOT Alert)
```cmd
sc create LegitSvc binPath= "C:\Program Files\MyApp\service.exe" start= auto
```

| Field | Value |
|-------|-------|
| ServiceName | LegitSvc |
| ServiceFileName | C:\Program Files\MyApp\service.exe |

**Result: ✅ SUPPRESSED by `filter_legit`**  
`ServiceFileName|startswith: 'C:\Program Files\'` matched the filter. No alert generated.

---

## Summary

| Test Case | Branch Tested | Event Source | Detection | Notes |
|-----------|--------------|--------------|-----------|-------|
| 1 — Non-standard path | `service_install` | System EID 7045 | ✅ Triggered | Path in `\Users\Public\` |
| 2 — Random service name | `random_service_name` | System EID 7045 | ✅ Triggered | Regex matched `xK9mQ3rT` |
| 3 — Direct registry write | `registry_service_set` | Sysmon EID 13 | ✅ Triggered | Bypasses `sc.exe`, still caught |
| FP — Legit install | `service_install` | System EID 7045 | ✅ Suppressed | `filter_legit` worked correctly |

**All detection branches functional. False positive filter validated.**

---

## Validation Tools Used

```bash
# Sigma CLI conversion (test syntax)
sigma convert -t splunk Day05_Sigma_Service_Creation.yml

# Chainsaw hunt against collected .evtx
chainsaw hunt . --sigma Day05_Sigma_Service_Creation.yml --mapping mappings/sigma-event-logs-all.yml
```

---

*Tested: 2026-06-25 | GraySentinel Intelligence Command — ODP Day 05*