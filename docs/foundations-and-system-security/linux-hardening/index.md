# Linux OS Hardening Lab — Operation Fortress Ledger

> Lab Type: Hands-on System Hardening  
> Tool: UFW, Fail2Ban, auditd  
> Framework: CIS Linux Hardening Benchmarks  
> System: Meridian Trust (Ubuntu Server 24.04 LTS)

---

## Lab Overview

This lab simulates a real-world post-deployment security audit on a Linux host codenamed **Meridian Trust**. Starting from an unhardened baseline — plaintext remote services, root-accessible SSH, no password policy, and zero log visibility — you will systematically reduce the attack surface, deploy active defense tooling, and validate every change against a measurable checklist.

The exercise is split into four phases: reconnaissance, hardening operations, incident-response drills, and a timed practical assessment. Each phase builds on evidence captured in the one before it, so missions must be completed in order.

!!! info "Environment"
This lab was performed on Ubuntu Server 24.04 LTS running in a disposable VM. Several steps disable services and change the SSH listening port — never run this against a production host.

---

## Learning Objectives

By the end of this lab, you will be able to:

- Baseline a Linux host and identify its real attack surface using native recon tools
- Apply core OS hardening controls: patch management, service minimization, SSH lockdown, firewalling, password policy, account hygiene, and file-permission control
- Deploy and verify active defense tooling (Fail2Ban, auditd) and centralized logging
- Diagnose and remediate realistic incident scenarios under time pressure
- Independently pass a practical, checklist-graded security validation

---

## Prerequisites

- Ubuntu Server 24.04 LTS VM with `sudo` access
- A second terminal/SSH session available before Mission 6
- Basic familiarity with `nano` or `vi`

---

## Rules of Engagement

1. Work on a disposable VM or snapshot — never a production host.
2. Every mission ends with a **Verify** step — confirm it before moving on.
3. Every mission has a 🚩 flag captured below as evidence.

---

## Phase 1 — Recon & Baseline Assessment

### Mission 1 — Fingerprint the Host

```bash
cat /etc/os-release
uname -a
```

![Host fingerprint output](../assets/img/linux-hardening/mission01-fingerprint.png)
_Figure 1: Confirming OS distribution, version, and kernel release before making any changes._

---

### Mission 2 — Map the Attack Surface

```bash
ss -tuln
systemctl list-units --type=service
```

![Open ports before hardening](../assets/img/linux-hardening/mission02-open-ports.png)
_Figure 2: Listening ports at baseline — note any unexpected service exposed here._

---

### Mission 3 — Audit the Human Attack Surface

```bash
cat /etc/passwd
awk -F: '($3>=1000)&&($1!="nobody"){print $1}' /etc/passwd
```

![Account audit](../assets/img/linux-hardening/mission03-account-audit.png)
_Figure 3: All accounts with UID ≥ 1000 and a valid login shell — the candidate list for Mission 9._

---

## Phase 2 — Hardening Operations

### Mission 4 — Patch the Foundation

```bash
sudo apt update && sudo apt upgrade -y
```

**Verify**

```bash
apt list --upgradable
```

![Patch verification](../assets/img/linux-hardening/mission04-patch-verify.png)
_Figure 4: No pending updates after patching._

---

### Mission 5 — Shut the Unnecessary Doors

```bash
sudo systemctl disable telnet
sudo systemctl stop telnet
```

**Verify**

```bash
systemctl status telnet
```

![Telnet disabled](../assets/img/linux-hardening/mission05-telnet-disabled.png)
_Figure 5: Telnet confirmed inactive (or absent) on the host._

---

### Mission 6 — Lock Down SSH

```bash
sudo nano /etc/ssh/sshd_config
```

Set:

```
PermitRootLogin no
PasswordAuthentication no
Port 2222
```

```bash
sudo systemctl restart ssh
```

!!! danger "Field Warning"
Confirm login on the new port from a second session before closing the first — this is the step most likely to lock you out.

**Verify**

```bash
grep PermitRootLogin /etc/ssh/sshd_config
grep PasswordAuthentication /etc/ssh/sshd_config
ss -tuln | grep 2222
```

![SSH configuration verified](../assets/img/linux-hardening/mission06-sshd-verify.png)
_Figure 6: Root login and password auth disabled; new port live._

![Successful SSH login on new port](../assets/img/linux-hardening/mission06-ssh-login-success.png)
_Figure 7: Key-based login confirmed working on port 2222 from a second session._

---

### Mission 7 — Raise the Wall

```bash
sudo ufw allow 2222/tcp
sudo ufw enable
sudo ufw deny 23
```

**Verify**

```bash
sudo ufw status verbose
```

![Firewall ruleset](../assets/img/linux-hardening/mission07-firewall-rules.png)
_Figure 8: UFW active, only port 2222 permitted._

---

### Mission 8 — Enforce Password Discipline

```bash
sudo nano /etc/login.defs
```

Set:

```
PASS_MAX_DAYS 90
PASS_MIN_DAYS 10
PASS_MIN_LEN 12
```

**Verify**

```bash
grep -E "PASS_MAX_DAYS|PASS_MIN_DAYS|PASS_MIN_LEN" /etc/login.defs
```

![Password policy verified](../assets/img/linux-hardening/mission08-password-policy.png)
_Figure 9: Password aging and length policy applied._

---

### Mission 9 — Neutralize Ghost Accounts

```bash
sudo userdel testuser
# or, if the account might be needed later:
sudo usermod -L testuser
```

**Verify**

```bash
cat /etc/passwd
```

![Accounts remediated](../assets/img/linux-hardening/mission09-accounts-remediated.png)
_Figure 10: Unrecognized accounts from Mission 3 removed or locked._

---

### Mission 10 — Seal the Crown Jewels

```bash
sudo chmod 600 /etc/shadow
```

**Verify**

```bash
ls -l /etc/shadow
```

![Shadow file permissions](../assets/img/linux-hardening/mission10-shadow-permissions.png)
_Figure 11: `/etc/shadow` locked to root-only access._

---

### Mission 11 — Deploy Active Defense

```bash
sudo apt install fail2ban -y
sudo systemctl enable --now fail2ban

sudo apt install auditd -y
sudo systemctl enable --now auditd
```

**Verify**

```bash
systemctl status fail2ban
systemctl status auditd
```

![Fail2Ban and auditd active](../assets/img/linux-hardening/mission11-fail2ban-auditd.png)
_Figure 12: Both active-defense services confirmed running._

---

### Mission 12 — Turn On the Cameras

```bash
sudo cat /var/log/auth.log
journalctl -xe
```

![Authentication log evidence](../assets/img/linux-hardening/mission12-auth-log-evidence.png)
_Figure 13: A real authentication event captured, confirming logging is active._

---

## Phase 3 — Incident Response Drills

### Drill A — The Open Port

**Symptom**

```bash
ss -tuln
# 0.0.0.0:23 LISTEN  <- unexpected
```

**Remediation**

```bash
sudo systemctl stop telnet
sudo ufw deny 23
ss -tuln
```

![Port 23 closed](../assets/img/linux-hardening/drillA-port23-closed.png)
_Figure 14: Port 23 confirmed closed after remediation._

---

### Drill B — The Weak SSH Config

**Symptom:** root login enabled over SSH, default port.

**Remediation:** disable root login and change the listening port (see Mission 6).

!!! tip "Reflection"
Changing the port alone does not fix this — an attacker who finds the new port still has an unauthenticated path to root unless password auth and root login are also disabled.

---

## Phase 4 — Practical Assessment

| Task   | Requirement                                  | Points |
| ------ | -------------------------------------------- | ------ |
| Task 1 | Identify and close all unused/at-risk ports  | 20     |
| Task 2 | Harden SSH (root login, password auth, port) | 20     |
| Task 3 | Enable firewall; allow only required ports   | 15     |
| Task 4 | Implement password policy                    | 15     |
| Task 5 | Verify system logs for unauthorized attempts | 15     |
| Task 6 | Deploy Fail2Ban and confirm it's running     | 15     |

**Scoring:** 90–100 audit-ready · 70–89 solid, revisit gaps · below 70 repeat Phase 2.

---

## Final Validation Checklist

```bash
sudo ufw status
grep PermitRootLogin /etc/ssh/sshd_config
ss -tuln
apt list --upgradable
grep PASS_MIN_LEN /etc/login.defs
ls -l /etc/shadow
systemctl status fail2ban
systemctl status auditd
```

| Check               | Expected Result            |
| ------------------- | -------------------------- |
| Firewall active     | `active`                   |
| SSH hardened        | `PermitRootLogin no`       |
| Ports minimized     | only required ports listed |
| Updates applied     | no pending updates         |
| Password policy set | `PASS_MIN_LEN` = 12        |
| Shadow file locked  | `-rw------- root root`     |
| Fail2Ban active     | `active (running)`         |
| Auditd active       | `active (running)`         |

![Final validation checklist](../assets/img/linux-hardening/final-validation-checklist.png)
_Figure 15: All eight hardening controls verified in a single session._

---

## Bonus Round — Advanced Hardening

- Enforce AppArmor in enforcing mode
- Deploy OSSEC or Wazuh and trigger a test alert
- Enable full-disk encryption with LUKS
- Disable USB mass storage via udev rules

---

## Learning Outcomes

By completing this lab, you have:

- Reduced a host's attack surface using measurable, verifiable controls rather than a checklist run blind
- Practiced recon-before-action, verify-after-every-change, evidence-as-you-go discipline
- Converted an unhardened host into one with a minimal attack surface, key-only access, active brute-force defense, and a real audit trail
- Built the muscle memory to diagnose a live misconfiguration under time pressure
