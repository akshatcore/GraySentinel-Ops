# FAQ Entry – GraySentinel Knowledge Base Proposal
**Proposed by:** Akshat Tiwari | MTS – Intelligence Command
**Date:** 2025-06-22
**Category:** Kali Linux / Nmap / Network Tools
**Priority:** High (Top repeated doubt)

---

## FAQ Entry #KB-001

### Q: Why does my nmap scan give "Failed to open interface" or "Operation not permitted"?

**Symptom:**
You run an nmap command on Kali Linux and receive one of the following errors:
```
Failed to open interface eth0. Error: Operation not permitted (1)
NSOCK ERROR: Failed to open interface "eth0"
```

---

**Before asking for help, check the following:**

**1. Are you running nmap with sudo?**

Raw packet scans (like `-sS` SYN scan) require root privileges. Run:
```bash
sudo nmap -sS <target_ip>
```
If you don't know your scan type, just add `sudo` and try again.

---

**2. Is your network interface actually up?**

Run:
```bash
ip a
```
Check that your interface (e.g., `eth0`, `ens33`) shows `state UP` and has an IP assigned.

If it's down, bring it up:
```bash
sudo ip link set eth0 up
sudo dhclient eth0
```

---

**3. Are your VM network settings correct?**

This is the most common hidden cause. In VirtualBox or VMware, your network adapter mode affects what nmap can reach:

| Mode | Use When |
|---|---|
| NAT | You need internet access from Kali |
| Bridged | You want Kali on the same LAN as your host |
| Host-Only | You're scanning between your host and VM only |
| Internal Network | Scanning between multiple VMs with no internet |

Check your adapter mode in your hypervisor settings and match it to your lab goal.

---

**4. Force nmap to use a specific interface:**
```bash
sudo nmap -e eth0 -sS <target_ip>
```

---

**5. Run a basic connectivity test first:**
```bash
ping -c 4 8.8.8.8
```
If ping fails, the issue is not nmap — it's your network configuration.

---

**Still stuck?**
Post your doubt in the group with:
- The exact command you ran
- The full error message
- Output of `ip a`
- Your hypervisor (VirtualBox / VMware) and adapter mode

We can't diagnose without evidence. 

---

**Tags:** `nmap` `kali-linux` `virtualbox` `vmware` `network` `beginner`
**Proposed Location:** GraySentinel Knowledge Base → Kali Linux → Nmap Troubleshooting

---

*This entry was drafted after observing repeated occurrences of this doubt in the group. Recommend pinning a short version in the group description or adding to the pinned Google Doc.*