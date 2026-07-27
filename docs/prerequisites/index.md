# Environment Setup — Installing Kali Linux and Metasploitable2

> Lab Type: Environment Setup (Prerequisite)  
> Tool: VMware Workstation Pro *or* Oracle VirtualBox  
> Duration: 45-90 minutes (depends on download speed)  
> System: Windows laptop (8 GB+ RAM), virtualization-capable CPU

---

## Overview

Before you can run any lab in this series, you need two virtual machines up and talking to each other:

- **Kali Linux** — your attacker machine, loaded with pentesting tools
- **Metasploitable2** — an intentionally vulnerable target machine

You can build this environment on **either VMware Workstation Pro or Oracle VirtualBox** — both are covered below in full, pick whichever you already have or prefer. Don't install both; either is sufficient on its own.

---

## Learning Objectives

By the end of this setup you will have:

- Confirmed your system meets the minimum requirements and has virtualization enabled
- Installed a hypervisor (VMware Workstation Pro or VirtualBox)
- Imported both the Kali Linux and Metasploitable2 virtual machines
- Configured networking so both VMs can reach each other (and, when needed, the internet)
- Verified connectivity end-to-end before starting any lab

---

## 1. System Requirements

| Requirement | Minimum |
| --- | --- |
| RAM | 8 GB or above (both VMs running together use roughly 3-4 GB) |
| Free disk space | 40 GB or above |
| OS | Windows laptop (Mac/Linux also work for VirtualBox; VMware Workstation is Windows/Linux only — Mac users should use VMware Fusion instead, same steps) |
| CPU | Must support hardware virtualization (Intel VT-x / AMD-V) |

### Enable virtualization (if it's currently disabled)

Most modern laptops ship with virtualization support but it's sometimes switched off in BIOS/UEFI by default. If your hypervisor refuses to power on a VM with an error mentioning VT-x/AMD-V, follow this guide to enable it yourself in your BIOS/UEFI settings, rather than filing an IT ticket and waiting:

**[How to Enable Intel VT-x in Your Computer's BIOS or UEFI Firmware](https://www.howtogeek.com/213795/how-to-enable-intel-vt-x-in-your-computers-bios-or-uefi-firmware/)**

!!! warning
    If you're on a work-managed/locked-down laptop, BIOS access may itself be restricted. In that case you genuinely do need IT — the self-help guide above only applies if you have BIOS access.

---

## 2. Choose Your Track

Both tracks get you to the same end state — pick one:

- **[Track A — VMware Workstation Pro](#track-a-vmware-workstation-pro)** — what this lab series was originally built and tested on
- **[Track B — Oracle VirtualBox](#track-b-oracle-virtualbox)** — free, no account required, good alternative if you'd rather not create a Broadcom account

---

## Track A: VMware Workstation Pro

### Objective

Install VMware Workstation Pro, then import both the Kali Linux and Metasploitable2 VMs.

### Step 1: Install VMware Workstation Pro

Since Broadcom's acquisition of VMware, Workstation Pro is **free for personal, educational, and commercial use** (version 17.5.2+), but downloading it requires a free Broadcom account — there's no anonymous public download link anymore.

1. Create a free Broadcom account: **[Broadcom account registration](https://profile.broadcom.com/web/registration)**
2. Once logged in, follow Broadcom's official download walkthrough: **[Download and license VMware Desktop Hypervisor (Workstation Pro / Fusion Pro)](https://knowledge.broadcom.com/external/article/368667/download-and-license-vmware-desktop-hype.html)**
3. Download **VMware Workstation Pro 17.5.2 or later** for Windows.
4. Run the installer, accept defaults. **No license key is required** — when prompted, select the free "Personal Use" option.

!!! warning
    Only versions 17.5.2 (Workstation) and above are available free. If you're offered anything older, that build requires a paid license — go back and select the current release instead.

### Step 2: Download and Import Kali Linux

1. Go to the official Kali VM download page: **[kali.org/get-kali — Virtual Machines](https://www.kali.org/get-kali/#kali-virtual-machines)**
2. Download the **VMware (64-bit)** image — a `.7z` archive.
3. Extract it (use [7-Zip](https://www.7-zip.org/) if you don't already have an extractor for `.7z`).
4. In VMware Workstation: **File → Open**, navigate into the extracted folder, and select the `.vmx` file inside it.
5. Power on the VM. If prompted "I copied it / I moved it," choose **I Copied It**.
6. Log in — default credentials: **username `kali`, password `kali`**.

Full official walkthrough with screenshots: **[Kali Docs — Import Pre-Made Kali VMware VM](https://www.kali.org/docs/virtualization/import-premade-vmware/)**

!!! warning
    Change the default password immediately after first login: `passwd`. Never leave a Kali VM on default credentials, especially once it has network/internet access.

### Step 3: Download and Import Metasploitable2

Metasploitable2 is distributed as a pre-built virtual disk, not an installer — there's no `.ova` for it officially, just a `.vmx`/`.vmdk` pair, which VMware can use directly.

1. Official download: **[SourceForge — Metasploitable2](https://sourceforge.net/projects/metasploitable/files/Metasploitable2/)** → `metasploitable-linux-2.0.0.zip` (~865 MB)
2. Extract the zip — you'll get a folder containing a `.vmx` file and several `.vmdk` files.
3. In VMware Workstation: **File → Open**, select the `.vmx` file from the extracted folder.
4. Power on. Default credentials: **username `msfadmin`, password `msfadmin`**.

!!! warning
    Metasploitable2 is deliberately full of unpatched vulnerabilities. Never bridge it to a real network or expose it to the internet — Host-only or NAT-only, always.

### Step 4: Network the Two VMs Together

1. For both the Kali VM and the Metasploitable2 VM: **right-click the VM → Settings → Network Adapter → select Host-only**.
2. Power on both VMs, then on each run:
   `ip a` (Kali) or `ifconfig` (Metasploitable2)
3. Confirm both landed on the same subnet (e.g. both `192.168.x.x` with the same first three octets).
4. From Kali, confirm you can reach the target:
   `ping -c 3 <metasploitable-ip>`

!!! warning
    Host-only has no internet access by design — that's intentional, it keeps your scans from ever accidentally reaching a real target. But steps that need the internet (e.g. `apt update`, downloading Nessus) will fail on Host-only. When you hit one of those, temporarily switch the adapter to **NAT**, do the internet-dependent step, then switch back to **Host-only** before scanning the target again.

!!! warning
    If one VM is set to Host-only and the other to NAT, they land on completely different subnets and can't see each other — you'll get `Network is unreachable`. Always double-check both VMs' Network Adapter settings match exactly.

---

## Track B: Oracle VirtualBox

### Objective

Install VirtualBox, then import both VMs using a shared NAT Network so they can reach each other and the internet.

### Step 1: Install Oracle VirtualBox

1. Download: **[VirtualBox — Official Downloads](https://www.virtualbox.org/wiki/Downloads)**
2. Get the **Windows hosts** package (or macOS/Linux, whichever matches your machine).
3. Run the installer with defaults. No account or license needed — VirtualBox is fully free and open-source.
4. Also install the **VirtualBox Extension Pack** from the same downloads page (adds USB 2.0/3.0 support and a few other features) — optional but recommended.

### Step 2: Download and Import Kali Linux

1. Go to: **[kali.org/get-kali — Virtual Machines](https://www.kali.org/get-kali/#kali-virtual-machines)** and download the **VirtualBox (64-bit)** `.7z` image (this is the current, actively maintained source — use this unless your team specifically hands you an older `.ova` to standardize on, see the note below).
2. Extract the `.7z` with 7-Zip. Inside, you'll find a `.vbox` file (or an `.ova`, depending on the release) plus disk files.
3. In VirtualBox: **File → Import Appliance**, browse to the extracted file, and follow the import wizard through to **Finish**.
4. Start the VM. Default credentials on current Kali releases: **username `kali`, password `kali`**.

!!! warning
    If you were specifically handed a `Kali-Linux-2019.3-vbox-amd64.ova` file (an older, pre-packaged release, sometimes distributed internally by a team for version consistency across a cohort) instead of downloading fresh from kali.org, its default login is different: **`root` / `toor`**, not `kali`/`kali` — Kali switched the default account around the 2020.1 release. Check with whoever gave you the file which version you're on before assuming credentials.

### Step 3: Download and Import Metasploitable2

Metasploitable2 has no official `.ova` — the official distribution is a `.vmdk` (VMware disk format), which VirtualBox can still use, just via a slightly different import path than Kali's OVA.

**Option 1 — Official source, manual disk attach:**

1. Download: **[SourceForge — Metasploitable2](https://sourceforge.net/projects/metasploitable/files/Metasploitable2/)** → `metasploitable-linux-2.0.0.zip`
2. Extract it — you'll get a `.vmdk` file.
3. In VirtualBox: **Machine → New**
4. Name it (e.g. `Metasploitable2`), Type: **Linux**, Version: **Other Linux (64-bit)**
5. When asked about the hard disk, choose **Use an Existing Virtual Hard Disk File** → browse to the extracted `.vmdk`
6. Finish. Power on. Default credentials: **username `msfadmin`, password `msfadmin`**.

**Option 2 — Pre-converted `.ova`, if your team provides one:**

If Manish or your team has already converted Metasploitable2 into an `.ova` and hosted it on an internal repo (as referenced in the setup notes you were given), use that instead — same **File → Import Appliance** flow as Kali, Step 2 above. Ask your team for that repo link directly, since it's an internal resource rather than a public one.

!!! warning
    Whichever option you use, never bridge Metasploitable2 to your real network. It's intentionally riddled with unpatched vulnerabilities — Host-only or a NAT Network only.

### Step 4: Set Up a Shared NAT Network

Unlike plain "NAT" mode (which isolates each VM so they *can't* see each other), VirtualBox's **NAT Network** mode lets every VM attached to it both reach the internet *and* see each other — exactly what you need here.

**Create the NAT Network (one-time, do this before configuring either VM):**

1. VirtualBox main window → **File → Preferences → Network**
2. Click **Add New NAT Network** (the `+` icon)
3. Leave the default name/settings as-is, click **OK**

**Attach both VMs to it:**

1. Select the Kali VM → **Settings → Network → Adapter 1** → **Attached to: NAT Network** → select the network you just created
2. Repeat for the Metasploitable2 VM
3. Power on both VMs

**Verify:**

On each VM, check its IP:
`ifconfig` (or `ip a` on newer Kali)

You should see an address in the `10.0.x.x` range on both VMs.

Confirm internet access from each VM:
`ping google.com`

Confirm the two VMs can see each other — from Kali:
`ping <metasploitable-ip>`

!!! warning
    If a VM shows "Attached to: NAT" (singular, no "Network") instead of "NAT Network," it's on the wrong mode — plain NAT won't let the two VMs reach each other. Double check the dropdown says **NAT Network** specifically, on both VMs.

---

## 3. Final Verification Checklist

Regardless of which track you used, confirm all of these before moving on to any lab:

- [ ] Kali VM boots and you can log in
- [ ] Metasploitable2 VM boots and you can log in (`msfadmin`/`msfadmin`)
- [ ] `ip a` / `ifconfig` on Kali shows an IP in the same subnet as Metasploitable2
- [ ] `ping -c 3 <metasploitable-ip>` from Kali succeeds
- [ ] Kali has internet access when you need it: `ping -c 3 8.8.8.8` (VMware: temporarily on NAT; VirtualBox: NAT Network handles this automatically)
- [ ] You've changed Kali's default password

---

## Discussion Questions

??? question "Why does Metasploitable2 need to be isolated from the internet?"

    It's intentionally loaded with unpatched, exploitable services (old FTP, SSH, Samba versions, planted backdoors, and more). If it were reachable from the internet, anyone could compromise it in minutes — Host-only/NAT Network keeps it reachable only from your own attacker VM.

??? question "Why does VMware's Host-only mode lack internet access, but VirtualBox's NAT Network doesn't?"

    They're solving slightly different problems. VMware's Host-only creates a private network limited to your host machine and its VMs, with no route out — maximally safe but requires manually switching modes when you need internet. VirtualBox's NAT Network is a virtual router that both connects VMs to each other *and* routes them out to the internet through NAT, so you get both without switching — the tradeoff is a very slightly larger (though still private) attack surface.

??? question "Why do different Kali versions have different default credentials?"

    Kali switched its default account from `root`/`toor` to a non-root `kali`/`kali` user starting with the 2020.1 release, to discourage the common but risky practice of using Kali as root for everyday use. If you're handed an older pre-2020 image, expect the old credentials.

---

## Learning Outcomes

Having completed this setup, you have:

- Verified your system meets minimum requirements and enabled virtualization if needed
- Installed a working hypervisor (VMware Workstation Pro or VirtualBox)
- Imported both the Kali Linux attacker VM and the Metasploitable2 target VM
- Configured an isolated network that lets both VMs reach each other without exposing the vulnerable target
- Confirmed end-to-end connectivity, ready to begin the lab series
