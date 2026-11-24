# Ticket 1 — New developer starts Monday, needs a test machine

## 1. What was asked

> From: Aigerim T. (Office Manager)
> Subject: test machine for new dev
>
> Hi, we have someone starting on Monday and he needs a test computer
> set up. Nothing special, just so he can try things without breaking
> anything. Can you write down how to do it so I can do the next one
> myself? Thanks

## 2. What was actually needed

| Missing from the request | What I would have asked | What I assumed |
|---|---|---|
| Which operating system(s) | Does the new developer work on Linux, Windows, or both? | Both **Ubuntu Server 26.04.1 LTS** and **Windows 11 Enterprise LTSC Evaluation**. A developer testing software commonly needs both platforms, and since I could not ask, covering both avoids a second build on Monday. |
| Physical computer vs virtual machine | Are we buying/issuing a separate physical PC, or running VMs on an existing laptop? | Two **virtual machines** in Oracle VirtualBox on an existing host. A physical PC costs money and cannot be reset in seconds. |
| Host hardware capacity | How much RAM, CPU, and free disk space does the host laptop have? | My host specs: **16 GB RAM**, **16 logical CPUs**, and **240 GB free disk space**. Guests are sized so the host stays responsive (see Section 3). |
| Network and connectivity | Does the test machine need internet access, and must the two guests reach each other? | Yes to both. Both VMs share one virtual **NAT Network** (`NatNetwork`). |
| Licensing and duration | Does the company own Windows licences, or is this a short-term test environment? | No retail or volume key is available. VirtualBox and Ubuntu are free; for Windows I used Microsoft's official **90-day Evaluation** edition, which is legal for testing without a product key. I also skipped the VirtualBox Extension Pack, whose licence allows only personal and educational use. |
| Reader's technical level | How technical is the person who will repeat this build next time? | A non-IT office manager who has never opened a hypervisor. Therefore `BUILD.md` starts from downloads, explains every choice, includes a **Check** after every step, and has a troubleshooting table. |

**Where the request is wrong or misleading:**
* **"Set up a test computer"** implies configuring a physical machine; a VM is cheaper, isolates experiments from the host OS, and resets in seconds.
* **"Nothing special, just so he can try things without breaking anything"** is contradictory: safe experimentation requires isolating the OS in a hypervisor and saving a **clean snapshot** before work begins, plus documenting how to roll back. Without a snapshot, breaking the guest still forces a full reinstall. Even a "basic" VM requires concrete decisions on memory, disk allocation, UEFI/TPM, firewall rules, and licensing.

## 3. What I did

1. Checked host capacity and hardware virtualisation, then installed Oracle VirtualBox `7.2.22`.
2. Created a shared virtual network (`NatNetwork`, `10.0.2.0/24`) with DHCP enabled and SSH port forwarding (`2222 -> 10.0.2.15:22`).
3. Built `lab01-ubuntu` (Ubuntu Server 26.04.1 LTS), verified system info and LVM disk layout, and created the `clean-install` snapshot.
4. Built `lab02-windows` (Windows 11 Enterprise LTSC Evaluation) with UEFI, Secure Boot, and virtual TPM 2.0, allowed inbound ICMPv4 ping in Windows Firewall, and created the `clean-install` snapshot.
5. Verified IP addresses, bidirectional ping between both guests, and internet reachability (`1.1.1.1`) from both guests.
6. Tested snapshot rollback and documented the full build and troubleshooting steps in [`BUILD.md`](BUILD.md).

**Settings that differ from VirtualBox defaults, and why:**

| Setting | Default | Mine | Why |
|---|---|---|---|
| Network mode (both VMs) | Plain `NAT` | `NAT Network` (`NatNetwork`) | Plain NAT isolates VMs from each other; Bridged is rejected by campus MAC filtering. NAT Network provides both internet access and VM-to-VM communication. |
| SSH port forwarding (`NatNetwork`) | None | Host `2222` → Guest `10.0.2.15:22` | Allows connecting to Ubuntu from the host terminal so commands and logs can be copied cleanly without console formatting glitches. |
| Unattended installation (both VMs) | Ticked (when ISO is selected) | Unticked | Prevents VirtualBox from auto-generating hidden credentials and keeps the installer steps predictable for a non-IT reader. |
| Ubuntu memory / CPUs | 1024 MB / 1 CPU | 2048 MB / 2 CPUs | 1 GB and 1 CPU is sluggish during updates and builds; 2 GB and 2 CPUs runs a headless server smoothly. |
| Ubuntu disk | 10 GB dynamic | 25 GB dynamic | Provides room for developer tools while using only `5.16 GB` on the host initially. |
| Ubuntu installation options | OpenSSH unticked, snaps optional | OpenSSH Server ticked, no snaps | OpenSSH enables remote terminal access; skipping snaps keeps the installation fast and lean. |
| Windows firewall & security | Varies by template | UEFI + Secure Boot + TPM `2.0` | Windows 11 Setup refuses to install without UEFI, Secure Boot, and TPM 2.0. |
| Windows Firewall rule | Inbound ICMPv4 blocked | Inbound ICMPv4 Echo allowed | Allows Ubuntu to ping Windows to verify connectivity without disabling the entire firewall. |

Windows memory (`4096 MB`), CPUs (`2`) and disk (`80 GB` dynamic) were left at the values VirtualBox proposes for Windows 11, so they are not in the table. They already meet Microsoft's minimum (4 GB RAM, 2 CPUs, 64 GB disk).

**Alternatives considered:**
* **NAT Network vs Bridged vs Plain NAT:** Plain NAT prevents the two guests from seeing each other. Bridged exposes each VM with its own MAC address on the physical LAN, which fails on the Innotech network due to MAC filtering. NAT Network hides both VMs behind the host IP while keeping them on `10.0.2.0/24`. Trade-off: external machines cannot initiate connections to the VMs without port forwarding.
* **Dynamic vs pre-allocated disks:** Pre-allocated disks offer slightly faster I/O but immediately claim all 105 GB (25 + 80 GB) on the host SSD. Dynamic disks currently use `5.16 GB` (Ubuntu) and `33.65 GB` (Windows, after installation and updates), preserving host storage.
* **Windows 11 Enterprise Evaluation vs consumer ISO:** A consumer ISO installed without a key runs unactivated and is not properly licensed for office use. The Evaluation edition is legally free for 90 days of testing; the trade-off is that it must be rebuilt or licensed after 90 days.
* **Ubuntu Server vs Desktop:** Server omits the graphical desktop, using noticeably less RAM and disk space than Desktop and leaving more headroom for Windows and the host.

**Resources, calculated from my host** (Maibenben X16C, AMD Ryzen processor, 16 logical processors, 16 GB RAM, 240 GB free disk space):
* **Memory:** `16 GB total − 6 GB used by host Windows at idle − 6 GB allocated to guests (2 GB Ubuntu + 4 GB Windows) = 4 GB free headroom` so the laptop does not swap or freeze when both VMs run simultaneously.
* **CPUs:** **2 vCPUs** (Ubuntu) + **2 vCPUs** (Windows) = **4 vCPUs** out of **16** logical processors on the host (leaving 12 logical processors for host tasks).
* **Disk:** Combined virtual limit is **25 GB + 80 GB = 105 GB** out of **240 GB** free. With dynamic allocation, actual use on the host SSD is about **39 GB** (`5.16 GB` for Ubuntu + `33.65 GB` for Windows).

**One thing that went wrong, and how I fixed it:**
1. **Windows Firewall blocking ping:** Ubuntu's first ping to Windows (`10.0.2.3`) timed out with 100% packet loss, although Windows could already ping Ubuntu. Windows blocks inbound ICMPv4 Echo by default. Adding a single inbound firewall rule (`New-NetFirewallRule`) fixed it without weakening firewall protection. I reproduced the failure afterwards by disabling and re-enabling the rule, and kept both outputs in `evidence/05b-firewall-problem.txt`.
2. **Ubuntu LVM default root size:** Choosing *Use an entire disk* with Ubuntu's default LVM layout allocated `12G` of the 25 GB virtual disk to the root partition (`/`) while leaving the rest unallocated in the volume group (shown by `df -h /` in `evidence/04-guest-uname.txt`). Since only `5.6G` is used initially, 12 GB is enough for testing, and I documented `sudo lvextend -r -l +100%FREE /dev/ubuntu-vg/ubuntu-lv` in `BUILD.md` so the full 25 GB can be claimed whenever needed.

## 4. Evidence

Every text file starts with `# Proves: ...` and keeps unedited command outputs; screenshots show the exact configuration screens.

* [evidence/00a-virtualbox-download.png](evidence/00a-virtualbox-download.png) — Official VirtualBox `7.2.22` download page (GPLv3 platform package; the Extension Pack next to it is not used).
* [evidence/00-create-nat-network.png](evidence/00-create-nat-network.png) — VirtualBox Network Manager showing `NatNetwork` (`10.0.2.0/24`, DHCP enabled) and SSH port forwarding (`2222 -> 10.0.2.15:22`).
* [evidence/01-new-vm-wizard.png](evidence/01-new-vm-wizard.png) — New VM wizard for `lab01-ubuntu` with Ubuntu Server ISO attached and unattended installation unticked.
* [evidence/01b-ubuntu-cpu-ram.png](evidence/01b-ubuntu-cpu-ram.png) — Hardware settings for `lab01-ubuntu`: `2048 MB` RAM, `2` CPUs, EFI unticked.
* [evidence/01c-ubuntu-disk.png](evidence/01c-ubuntu-disk.png) — Virtual hard disk creation for `lab01-ubuntu`: `25.00 GB` VDI with *Pre-allocate Full Size* unticked.
* [evidence/02-disk-settings.png](evidence/02-disk-settings.png) — Storage properties confirming `lab01-ubuntu.vdi` is dynamically allocated (`25.00 GB` virtual size, `5.16 GB` actual size).
* [evidence/02b-windows-disk.png](evidence/02b-windows-disk.png) — Storage properties of `lab02-windows.vdi`: `80.00 GB` virtual size, dynamically allocated, `33.65 GB` actually used. The other entries in the list (`lab04-win.vdi`, `VBoxGuestAdditions.iso`) belong to later work and are not part of this ticket.
* [evidence/03-network-adapter.png](evidence/03-network-adapter.png) — Network adapter settings for `lab01-ubuntu` attached to `NatNetwork` (MAC `0800271ED727`, matching `05`).
* [evidence/04-guest-uname.txt](evidence/04-guest-uname.txt) — Unedited terminal commands and output (`uname -a`, `lsb_release -a`, `hostname`, `df -h /`) confirming Ubuntu 26.04.1 LTS on `lab01-ubuntu`.
* [evidence/05-ping-between-guests.txt](evidence/05-ping-between-guests.txt) — Unedited commands and output from Windows (`10.0.2.3`) and Ubuntu (`10.0.2.15`) showing bidirectional ping with 0% loss and internet reachability (`1.1.1.1`).
* [evidence/05b-firewall-problem.txt](evidence/05b-firewall-problem.txt) — Linux→Windows ping with the Windows firewall rule disabled (fails) and enabled (works); reproduced afterwards, unedited.
* [evidence/06-snapshot-list.png](evidence/06-snapshot-list.png) — VirtualBox Snapshots panel for `lab01-ubuntu` showing the `clean-install` snapshot.
* [evidence/06b-snapshot-list-windows.png](evidence/06b-snapshot-list-windows.png) — VirtualBox Snapshots panel for `lab02-windows` showing the `clean-install` snapshot.
* [evidence/07a-windows-wizard.png](evidence/07a-windows-wizard.png) — New VM wizard for `lab02-windows` with the Windows 11 Enterprise LTSC Evaluation ISO attached and unattended installation unticked.
* [evidence/07-windows-vm-settings.png](evidence/07-windows-vm-settings.png) — System settings for `lab02-windows` showing `4096 MB` RAM, UEFI, Secure Boot, and TPM `2.0` enabled.
* [evidence/07b-windows-network.png](evidence/07b-windows-network.png) — Network settings for `lab02-windows` attached to `NatNetwork`.
* [evidence/07c-windows-installer.png](evidence/07c-windows-installer.png) — Completed Windows 11 Enterprise LTSC Evaluation desktop running inside VirtualBox.

## 5. What I would do differently

In my initial attempt, copying text directly from the VirtualBox console window caused formatting corruption in my ping log, and I had not structured the report around a non-technical reader from the start. Next time, I would configure SSH port forwarding on `NatNetwork` (`2222 -> 10.0.2.15:22`) immediately after installing Ubuntu so I can connect from my host terminal and save clean, unedited logs directly into `evidence/` without console copy-paste glitches. I would also capture step-by-step screenshots during the initial build rather than recreating wizard screens afterward.
