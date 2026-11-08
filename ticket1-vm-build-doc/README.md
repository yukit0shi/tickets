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

**The real reader of the deliverable is Aigerim, not the developer.** She is an office manager and will repeat the build herself, so `BUILD.md` assumes no IT knowledge: it starts with the downloads, explains every choice in one or two sentences, has a **Check** line after every step, a troubleshooting table and a separate rollback section. The developer only uses the result.

**"Test computer" does not mean a physical computer.** The request sounds like buying or configuring a PC, but "try things without breaking anything" is a requirement for isolation and undo. A virtual machine on an existing laptop gives both: it needs no new hardware, and a snapshot returns it to a clean state in seconds. I assumed a VM in Oracle VirtualBox.

**"Nothing special" hides several decisions the request does not make.** What I assumed instead of an answer:

| Left out | What I assumed |
|---|---|
| Which operating system the developer needs | Both Linux (Ubuntu Server 26.04.1 LTS) and Windows 11, because the course brief asks for both and the request does not say |
| Host RAM and disk | My laptop: 16 GB RAM, 240 GB free. Guests sized from these figures (section 3) |
| Internet access and whether the machines must see each other | Yes to both; one shared NAT Network |
| Licences | Ubuntu and VirtualBox are free. Windows needs a licence. I used Windows 11 Enterprise LTSC **Evaluation**, which is free but expires after 90 days. For a permanent machine the company must buy a licence; this is a decision for the manager, not the technician |
| Rollback ("without breaking anything") | A snapshot named `clean-install` on each VM, with a documented restore procedure |
| Who will rebuild it | Aigerim, with no IT background, which is why the guide includes downloading VirtualBox and the ISOs and a troubleshooting table |

**What I would have asked Aigerim:**
1. Does the developer work on Linux, Windows or both?
2. Does the company already have Windows licences, or is a 90-day evaluation acceptable?
3. How much RAM and free disk does the laptop that will host the VM have?
4. Must the VM reach company shares or only the internet?
5. Is the machine for one week of testing or for permanent use? That decides whether a licence and updates matter.

**Where the request was wrong or misleading:** "set up a computer" suggests a physical one; a VM is cheaper, faster to reset and safer. "Nothing special" is also wrong: even a "simple" VM needs about ten decisions (sizing, networking, licences, rollback), and the document has to make them for a reader who cannot.

## 3. What I did

Steps in order are in [`BUILD.md`](BUILD.md) (19 steps, troubleshooting, rollback). In short: installed VirtualBox, created a shared `NatNetwork`, built `lab01-ubuntu` and `lab02-windows`, allowed ping in the Windows firewall, tested both directions and the internet, took a `clean-install` snapshot of each machine.

**Settings that differ from VirtualBox's defaults, and why:**

| Setting | Value | Why |
|---|---|---|
| Network (both) | NAT Network `NatNetwork` instead of NAT | Plain NAT hides each VM from the other; the campus network filters Bridged adapters by MAC |
| Ubuntu memory / CPUs | 2048 MB / 2 | Default 1024 MB and 1 CPU is slow for development; 2 GB is enough for Server |
| Ubuntu disk | 25 GB, dynamic | Real size at the start is about 5 GB (`evidence/02-disk-settings.png`) |
| Windows memory / CPUs / disk | 4096 MB / 2 / 64 GB dynamic | Windows 11 minimum requirements |
| Windows EFI, Secure Boot, TPM | On, TPM v2.0 | The Windows 11 installer refuses to continue without them |
| Windows firewall | Inbound ping (ICMPv4) allowed | Otherwise Ubuntu cannot ping Windows |

**Resources calculated from the host:** 16 GB RAM. Ubuntu 2 GB + Windows 4 GB = 6 GB, which leaves 10 GB (62%) for the laptop. Disk: the limits add up to 25 + 64 = 89 GB, but both disks are dynamic: Ubuntu was 5.16 GB in use at the start, Windows ~22 GB, against 240 GB free on the host.

**Decisions and alternatives:**
- **NAT Network vs Bridged.** Bridged would let the VM be reached from other computers, but the Innotech network filters by MAC address and rejects the VM's own MAC. NAT Network passes traffic through the host's address and still lets the two VMs talk. The cost: nothing outside the laptop can reach the VMs without port forwarding.
- **Dynamic vs pre-allocated disk.** Pre-allocation is slightly faster at runtime but takes all 89 GB at once. For a test machine on a shared laptop, saving space matters more than speed.
- **Windows 11 Enterprise Evaluation vs a regular Windows 11 ISO.** A regular ISO installs without a key and has no time limit, but runs unactivated with a watermark and is not licensed for use. The Evaluation version is officially free and licensed for testing, but expires after 90 days. I chose it because the ticket is a test machine and I did not want to hand over an unlicensed system.
- **Ubuntu Server vs Desktop.** Server uses far less memory and disk and has no graphical interface; a developer who needs a desktop can add one later.
- **Allowing ping vs switching the firewall off.** One rule opens only ICMP echo; switching the firewall off removes all protection.

**One thing that went wrong:** the Ubuntu installer's default layout (LVM) gave the root partition 12 GB of the 25 GB disk, as `df -h /` shows in `evidence/04-guest-uname.txt`. I did not expect this and it would surprise anyone who looked at the disk size and the free space. I left it, because a test machine uses about 5.6 GB, and noted here that the rest remains free in the volume group and can be added with `sudo lvextend -r -l +100%FREE /dev/ubuntu-vg/ubuntu-lv`.

## 4. Evidence

- `01-new-vm-wizard.png` — Ubuntu wizard: name `lab01-ubuntu`, Linux / Ubuntu (64-bit), ISO selected.
- `01b-ubuntu-cpu-ram.png` — Ubuntu has 2048 MB memory and 2 CPUs (non-default settings).
- `01c-ubuntu-disk.png` — 25 GB disk, "pre-allocate" not selected.
- `02-disk-settings.png` — the disk is dynamic: virtual size 25 GB, actual 5.16 GB.
- `03-network-adapter.png` — adapter set to NAT Network `NatNetwork`; the MAC matches the one in `05`.
- `04-guest-uname.txt` — `uname`, `lsb_release`, `hostname` and `df` from the guest: a working Ubuntu 26.04.1 LTS. Text, not edited.
- `05-ping-between-guests.txt` — `ipconfig` / `ip addr` and ping output from both guests: they share the subnet `10.0.2.0/24` (Windows `10.0.2.3`, Ubuntu `10.0.2.15`), see each other in both directions with 0% loss, and both reach `1.1.1.1`. The file is assembled from two terminals (Windows PowerShell and Ubuntu); the output inside was not edited. Reaching each other proves the mode is NAT Network, because plain NAT cannot do that.
- `06-snapshot-list.png` — `clean-install` snapshot of `lab01-ubuntu`.
- `06b-snapshot-list-windows.png` — `clean-install` snapshot of `lab02-windows`.
- `07-windows-vm-settings.png` — Windows VM: 4096 MB, EFI, Secure Boot, TPM 2.0.

## 5. What I would do differently

My first submission had a ping log with lines I had edited by hand because I could not easily copy text from the VirtualBox console. Next time, I would set up SSH port forwarding from the very beginning to log into the Ubuntu machine from my host terminal. This allows recording every command in a terminal log cleanly without ever touching the output manually. I would also make sure to address the non-technical reader's perspective in the documentation right from the start.
