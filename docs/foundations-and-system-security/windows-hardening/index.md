# Windows OS Hardening Lab - Operation: Clean Endpoint — GUI Hardening Guide

---

> Lab Type: Hands-on System Hardening  
> Tool: lusrmgr.msc, secpol.msc, gpedit.msc, services.msc, eventvwr.msc  
> Framework: Windows Security app  
> System: Windows 10, 11

---

## Lab Overview
This lab serves as a comprehensive, click-path companion to the PowerShell version of the Operation: Clean Endpoint lab. It walks students through the manual process of endpoint hardening using the Windows Graphical User Interface (GUI) rather than the command-line console.  

Designed for students who are less comfortable navigating a terminal, or as a visual reference to understand what underlying system parameters actually change when a script is executed, this lab systematically remedies common audit vulnerabilities. 

Across five core modules and an optional hardening section, students utilize native Windows administration tools—such as Local Users and Groups (lusrmgr.msc), Local Security Policy (secpol.msc), and Local Group Policy Editor (gpedit.msc)—to establish a resilient security baseline on a Windows endpoint.

---

## Lab Objectives
1. Mitigate Identity and Authentication Risks: Rename and disable the built-in Windows Administrator account, verify the Guest account status, and enforce strict local password and lockout policies.  
2. Deploy Core Host Defenses: Verify Microsoft Defender Real-Time Protection, validate stateful firewall profiles, and configure advanced Attack Surface Reduction (ASR) rules via Group Policy to mitigate "living-off-the-land" execution techniques.  
3. Implement Data-at-Rest Encryption: Verify Trusted Platform Module (TPM) readiness and provision BitLocker Drive Encryption using the XTS-AES algorithm on the primary system volume.  
4. Reduce Local Attack Surface: Disable legacy features and services (such as SMBv1 and Remote Registry) and secure User Account Control (UAC) prompts on the Secure Desktop.  
5. Enhance Detection and Forensic Readiness: Configure fine-grained Advanced Audit Policies, enable full PowerShell Script Block Logging, expand command-line auditing, and adjust the Windows Security Event Log capacity for long-term retention.  
6. Implement Advanced Defensive Controls: Apply system-wide exploit protections, enable anti-ransomware folder access controls, and disable vulnerable legacy protocols like LLMNR, NetBIOS, and PowerShell v2

---

## Milestone 1 - Account & Authentication Hardening
Audit finding: Built-in Administrator enabled, no lockout policy, weak password requirements, Guest status unknown.

### Rename & Disable Built-in Administrator
1. Right-click Start → Computer Management.
![reports tab](assets\1-open-comp-man.png)
2. Expand Local Users and Groups → Users.
![reports tab](assets\2-local-users-and-groups.png)
3. Right-click Administrator → Properties, tick Account is disabled, click OK.
![reports tab](assets\3-admin-acc-disbled.png)
4. Right-click Administrator → Rename, type svc-admin-legacy , press Enter.

!!! warning "Why?"
    The built-in Administrator SID (ending in -500 ) is always the same on every Windows box, so attackers don't need to guess a username — they just brute-force the password. Disabling it removes that account as a target entirely; renaming it is a secondary layer in case someone re-enables it.

###  Confirm Guest Account is Disabled
1. Same Local Users and Groups → Users list.
2. Right-click Guest → Properties, confirm Account is disabled is ticked (it usually is by
default, but audits shouldn't assume).
![reports tab](assets\4-guest-acc-disabled.png)

!!! warning "Why?"
    Guest is an unauthenticated entry point. Even if disabled by default on modern builds, imaged/cloned machines sometimes inherit non-default states — always verify, don't assume.

### Account Lockout Policy
1. Open secpol.msc → Account Policies → Account Lockout Policy.
2. Set Account lockout threshold = 5 invalid logon attempts .
3. Windows will prompt to auto-set duration/reset window — accept, or set both to 15
minutes manually.
![reports tab](assets\5-secpol-msc.png)

!!! warning "Why?"
    Without a lockout policy, an attacker with local or RDP access can brute-force passwords indefinitely.

![reports tab](assets\6-account-lockout.png)

###  Password Policy
1. Same window, Account Policies → Password Policy.
2. Set:
Minimum password length = 14
Maximum password age = 90
Minimum password age = 1
Enforce password history = 5

!!! warning "Why?"
    Length matters more than complexity rules against modern cracking hardware. Minimum age (1 day) stops users from cycling through history 5 times in a row to get back to their old password immediately.

### Create a Standard Day-to-Day User
1. Computer Management → Local Users and Groups → Users, right-click blank space
→ New User.
2. Username labuser , set a password, untick "User must change password at next
logon" if this is a lab image, click Create.
3. By default new users land in the Users group (standard, non-admin) — confirm under
the account's Properties → Member Oftab that it does not sayAdministrators.

!!! warning "Why?"
    Principle of least privilege — daily browsing/email/office work shouldn't run in an admin context, since malware inherits the privilege level of whoever's logged in.

??? question "Checkpoint"
    Computer Management → Local Users and Groups → Users. Confirm no account named "Administrator," Guest and svc-admin-legacy show as disabled (grey icon with down-arrow), labuser exists and is enabled.

## Milestone 2 - Defender, Firewall & ASR Rules
Audit finding: Defender real-time protection unverified, firewall profiles unconfirmed, no
ASR rules applied.

### Verify & Enable Real-Time Protection
1. Open Windows Security → Virus & threat protection.
![reports tab](assets\7-virus-threat-manage-settings.png)
2. Under Virus & threat protection settings, click Manage settings.
3. Toggle Real-time protection to On (also enable Cloud-delivered protection and
Automatic sample submission here — covers step 2.2 as well).
![reports tab](assets\8-real-time-protection-on.png)

!!! warning "Why?"
    Real-time protection is sometimes disabled by third-partyAV installs that never got cleanly removed, or by an initial deployment image. Cloud-delivered protection gets you signature updates faster than the local definition cycle.

### Cloud Protection & Sample Submission
Done in the same panel as above (Cloud-delivered protection / Automatic sample submission toggles).

!!! warning "Why?"
    Feeds Microsoft's threat intelligence pipeline with real samples from the field, which improves detection for zero-days across the whole Defender install base — including yours.

### Confirm All Firewall Profiles Are On
1. Windows Security → Firewall & network protection.
![reports tab](assets\9-windows-firewall.png)
2. Click into Domain network, Private network, and Public network one at a time.
3. Confirm Microsoft Defender Firewall is On for each.
4. For stricter default-deny behavior: Control Panel → Windows Defender Firewall →
Advanced settings, right-click each profile node (Domain/Private/Public Firewall
Properties) → confirm Inbound connections: Block (default).
![reports tab](assets\10-firewall-on.png)

!!! warning "Why?"
    A profile can be silently off if it was disabled for a one-time troubleshooting session and never re-enabled. Public profile especially matters on laptops that connect to coffeeshop/airport Wi-Fi.

### Enable Core Attack Surface Reduction (ASR) Rules
ASR rules don't have a friendly toggle UI on unmanaged (non-Intune) machines — the
closest GUI path is via Local Group Policy Editor (Pro/Enterprise):
1. gpedit.msc → Computer Configuration → Administrative Templates → Windows
Components → Microsoft Defender Antivirus → Microsoft Defender Exploit Guard →
Attack Surface Reduction.
2. Double-click Configure Attack Surface Reduction rules → Enabled.
3. Click Show... next to Options, and add each rule ID with value 1 (Block mode):
D4F940AB-401B-4EFC-AADC-AD5F3C50688A — Block Office apps from creating child
processes
9E6C4E1F-7D60-472F-BA1A-A39EF669E4B2 — Block credential stealing from LSASS
BE9BA2D9-53EA-4CDC-84E5-9B1EEE46550F — Block executable content from
email/webmail
75668C1F-73B5-4CF0-BB93-3ECF5CB7CC84 — Block Office apps from injecting into
other processes

!!! warning "Why?"
    ASR rules block specific "living-off-the-land" techniques — legitimate Windows/Office functionality abused by malware — that traditional signature-based AV often misses because no malicious file ever touches disk.

??? question "Checkpoint"
    Windows Security home screen — green checkmarks across Virus & threat protection and Firewall & network protection tiles; gpedit ASR rule list shows all 4 rules set to Block.

## Milestone 3 - BitLocker & Data-at-Rest Protection
Audit finding: Drive not encrypted.
Requires Windows 11 Pro or Enterprise + TPM. Home edition doesn't expose full
BitLocker in the GUI.

###  Check TPM Status
1. Press Win+R, type tpm.msc , Enter.
2. Confirm "The TPM is ready for use" and note the spec version (2.0 preferred).
![reports tab](assets\11-trm.png)

### Enable BitLocker on System Drive
1. Control Panel → System and Security → BitLocker Drive Encryption.
2. Next to drive C:, click Turn on BitLocker.
![reports tab](assets\12-bitlocker-drive-encryption.png)
3. Choose how to unlock at startup — TPM-only is fine for a managed corporate laptop.
4. Save your recovery key — choose "Save to your Microsoft account" (if managed) or
"Print/Save to file" for a lab environment (step 3.3).
5. Choose Encrypt used disk space only (faster, fine for new-ish machines) or Encrypt
entire drive (preferred for machines that have held unencrypted data before).
![reports tab](assets\13-c-drive-bitlocker.png)
![reports tab](assets\14-encrypt-drive.png)
![reports tab](assets\15-drive-encrypting.png)
6. Choose New encryption mode (XTS-AES).
7. Click Start encrypting.

!!! warning "Why?"
    XTS-AES 256: stronger and more tamper-resistant than the legacyAES-CBC mode; XTS is specifically designed for disk-sector encryption.

### Recovery Key Backup
Handled in the wizard above. In production, never let the only copy of the recovery key live
on the encrypted drive itself — escrow it to Active Directory or Entra ID/Intune so IT can
recover a locked-out machine.

### Monitor Encryption Progress
Control Panel → BitLocker Drive Encryption shows a live percentage next to drive C: while encryption runs in the background.

??? question "Checkpoint"
    BitLocker Drive Encryption panel shows "Encryption in progress" or "BitLocker on" next to C:, and a recovery key was saved somewhere off the encrypted volume.

## Milestone 4 - Local Security Policy & Attack Surface Reduction
Audit finding: SMBv1 enabled, unnecessary services running, weak UAC/LSA settings.

### Disable SMBv1
1. Control Panel → Programs → Turn Windows features on or off.
2. Untick SMB 1.0/CIFS File Sharing Support (expand it and untick the sub-items too).
3. Click OK, reboot when prompted.
![reports tab](assets\16-turn-off-smb.png)

!!! warning "Why?"
    SMBv1 is 30+ years old, has no meaningful protection against modern attacks, and was the transport for EternalBlue/WannaCry-style worms. There's essentially no legitimate reason to keep it enabled today.

### Harden UAC to Secure Desktop
1. Start menu → search "Change UserAccount Control settings".
2. Drag the slider to the top: "Always notify".
3. Click OK.
![reports tab](assets\17-notify-changes.png)

!!! warning "Why?"
    This forces UAC prompts onto the secure desktop (a locked-down rendering layer other processes can't draw over), which prevents malware from spoofing a fake "Yes" button on a UAC dialog.

### Enable LSA Protection (RunAsPPL)
No clean GUI toggle exists for this — closest option:
1. gpedit.msc → Computer Configuration → Administrative Templates → System → Local
Security Authority , or
2. Windows Security → Device security → Core isolation — enable Local Security
Authority protection if the option is present (depends on hardware/driver support).

If neither is available, this genuinely requires the registry key
( HKLM\SYSTEM\CurrentControlSet\Control\Lsa\RunAsPPL = 1 ) — worth telling students that
not everything in Windows has a GUI path, and that's a legitimate limitation to flag.

!!! warning "Why?"
    Runs LSASS as a Protected Process, which blocks common credential-dumping tools (like Mimikatz) from reading its memory even with local admin rights.

### Disable Legacy Services
1. services.msc .
2. Find Remote Registry, Fax, Windows Media Player Network Sharing Service.
3. For each: right-click → Properties, click Stop, set Startup type to Disabled, OK.
![reports tab](assets\18-remote-registry.png)

!!! warning "Why?"
    Unused services are unnecessary attack surface — nobody's faxing anything, and Remote Registry lets a remote (even unauthenticated in older configs) user query and modify registry keys.

### Enable PowerShell Script Block Logging
1. gpedit.msc → Computer Configuration → Administrative Templates → Windows
Components → Windows PowerShell .
2. Double-click Turn on PowerShell Script Block Logging → Enabled → OK.

!!! warning "Why?"
    Logs the actual decoded contents of PowerShell commands run on the machine — including obfuscated/base64-encoded one-liners a lot of malware uses — giving you something to actually investigate later instead of just "PowerShell.exe ran."

??? question "Checkpoint"
    Windows Features shows SMB1 unticked (post-reboot), Services console shows the three services as Disabled/Stopped, UAC slider at top.

## Milestone 5 - Auditing, Event Logging & Detection Readiness
Audit finding: No advanced audit policy configured.

### Enable KeyAudit Subcategories
The basic secpol.msc → Local Policies → Audit Policy node only has coarse "Legacy"
settings. For the fine-grained subcategories used in the lab, you need Advanced Audit
Policy Configuration:
1. secpol.msc → Security Settings → Advanced Audit Policy Configuration → System
Audit Policies .
2. Logon/Logoff → Logon: double-click, tick Configure the following audit events,
check both Success and Failure.
3. Logon/Logoff → Special Logon: Success + Failure.
4. Detailed Tracking → Process Creation: Success.
5. Account Management → Security Group Management: Success + Failure.
6. Account Logon → Credential Validation: Success + Failure.

!!! warning "Why?"
    The default audit policy barely logs anything useful. These subcategories cover the events you actually need during an incident: who logged on and how, what processes spawned, and whether anyone was added to a privileged group.

### Full Command-Line Auditing
1. gpedit.msc → Computer Configuration → Administrative Templates → System → Audit
Process Creation .
2. Double-click Include command line in process creation events → Enabled → OK.

!!! warning "Why?"
    "A process was created" tells you almost nothing. The command line tells you what it actually did — e.g. powershell.exe -enc <base64> is a very different story than powershell.exe alone.

### Increase Security Log Size
1. eventvwr.msc → Windows Logs → Security , right-click → Properties.
2. Set Maximum log size (KB) to 1048576 (1 GB).
3. Confirm "Overwrite events as needed" (or archive, depending on your retention
needs) is selected.
![reports tab](assets\20-event-viewer.png)

!!! warning "Why?"
    The default Security log size is small enough that a single busy day can roll it over, silently destroying your forensic trail exactly when you need it.

### Verify Event Capture
1. eventvwr.msc → Windows Logs → Security .
2. Use the Filter Current Log action on the right, filter by Event ID 4624 (successful
logon).
3. Confirm recent logon events appear with plausible timestamps.

??? question "Checkpoint"
    Advanced Audit Policy node shows the five subcategories configured
    for Success/Failure as listed above; Event Viewer's Security log returns real 4624 events
    when filtered.

## Extra Hardening Worth Adding (Beyond the Original 5 Modules)

### Exploit Protection (system-wide DEP/ASLR/CFG)
Windows Security → App & browser control → Exploit protection settings. Confirm Data Execution Prevention (DEP), Address Space Layout Randomization (ASLR — Force mandatory), and Control Flow Guard (CFG) are all On at the system level. Why: These are memory-corruption exploit mitigations baked into Windows since Vista/7-era, but they can be disabled per-app by installers or left off system-wide on older images.

### Controlled FolderAccess (anti-ransomware)
Windows Security → Virus & threat protection → Ransomware protection → Controlled folder access → On. Why: Blocks unrecognized processes from writing to protected folders (Documents, Desktop, Pictures by default) — a direct mitigation against ransomware encrypting files, independent of whether Defender's signatures caught the binary itself.

### Disable LLMNR and NetBIOS Name Resolution
LLMNR: gpedit.msc → Computer Configuration → Administrative Templates → Network → DNS Client → Turn off multicast name resolution → Enabled . NetBIOS: Network adapter → Properties → IPv4 → Advanced → WINS tab → Disable NetBIOS over TCP/IP. Why: Both protocols answer name-resolution requests on the local broadcast segment with no authentication, which is the basis of the classic Responder/LLMNR-poisoning attack used to capture NTLM hashes from other machines on the same network.

### Secure Boot + Virtualization-Based Security / Credential Guard
Windows Security → Device security → Core isolation → Memory integrity → On. Secure Boot itself is a BIOS/UEFI-level setting, not Windows — check it in firmware setup. Why: Memory integrity (HVCI) runs core Windows processes in a hypervisor-protected container, and Credential Guard (same section, on Enterprise) isolates credential material even from a fully-compromised kernel.

### Disable AutoRun/AutoPlay
Settings → Bluetooth & devices → AutoPlay → Off (toggle at top). Why: Closes the classic
"malicious USB drive" attack path where inserting a drive auto-launches something.

### Restrict Remote Desktop
Settings → System → Remote Desktop. If not needed, leave off. If needed, enable Require
devices to use Network Level Authentication (NLA) under the advanced settings, and
restrict which users can connect via System Properties → Remote → Select Users. Why:
RDP without NLA lets an unauthenticated connection reach the logon screen, expanding
attack surface for credential-stuffing and known RDP-specific CVEs.

### Windows Update Configuration
Settings → Windows Update → Advanced options. Confirm automatic updates are on, and
enable "Receive updates for other Microsoft products" to keep Office/Defender patched
alongside the OS. Why: A hardening pass is a snapshot in time; unpatched software erodes
it within weeks. This is the one control that keeps working after the lab ends.

### Disable PowerShell v2
Control Panel → Programs → Turn Windows features on or off → untick Windows
PowerShell 2.0. Why: PowerShell v2 doesn't support Script Block Logging orAMSI
(Antimalware Scan Interface) — attackers with local access can explicitly invoke it
( powershell -version 2 ) to run malicious scripts completely invisibly to the logging you
just configured in Module 5.

| # | Check | Where to Verify |
| :--- | :--- | :--- |
| **1** | Guest account disabled | Computer Management → Local Users and Groups |
| **2** | Defender real-time protection on | Windows Security → Virus & threat protection |
| **3** | Firewall on for all 3 profiles | Windows Security → Firewall & network protection |
| **4** | SMBv1 disabled | Turn Windows features on or off |
| **5** | BitLocker on C: | Control Panel → BitLocker Drive Encryption |
| **6** | LSA / Core isolation protections on | Windows Security → Device security |
| **7** | ASR rules active | `gpedit.msc` → ASR node |
| **8** | Advanced audit subcategories configured | `secpol.msc` → Advanced Audit Policy |
| **9 (new)** | Controlled folder access on | Windows Security → Ransomware protection |
| **10 (new)** | LLMNR/NetBIOS disabled | `gpedit.msc` / adapter WINS settings |

Reboot required after: SMBv1 removal, LSA protection change. Flag this the same way the
original deck does — students will ask why their checkpoint "failed" if they skip the reboot.

## Lab Outcomes
Upon successful completion of the lab exercises, students will have achieved the following deliverables:
1. Secured Local Accounts: An endpoint architecture where the default Administrator SID (-500) is obfuscated and disabled, the Guest account is restricted, and a standard, non-privileged day-to-day user profile is actively deployed.  
2. Active Threat Mitigation: A fully operational Microsoft Defender environment with green status indicators across real-time, cloud-delivered, and firewall profiles, bolstered by four strict ASR rules running in Block mode.  
3. Encrypted Volume Architecture: A system drive fully encrypted via BitLocker (XTS-AES 256) with a verified recovery key safely escrowed off the primary volume.  
4. Hardened System Configuration: A machine stripped of legacy protocols (SMBv1, NetBIOS, LLMNR) and risky services (Remote Registry, Fax), featuring isolated Local Security Authority (LSA) memory protection and highly restrictive UAC behaviors.  
5. Incident Response-Ready Logging: A robust event-logging configuration featuring a 1GB Security log capacity capturing detailed process creation strings, PowerShell script execution blocks, and advanced success/failure authentication events (Event ID 4624).  