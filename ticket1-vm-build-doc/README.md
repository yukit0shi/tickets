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
The request from the Office Manager asked for a "test computer" but omitted critical technical requirements necessary for a hypervisor build. It did not specify which operating system the new developer needs (Linux or Windows), how much RAM and storage should be allocated, or whether the machine requires external internet access and network isolation
Instead of delaying the setup to ask for clarification, I assumed the developer might need to test cross-platform environments. Therefore, I built two guest virtual machines: one Ubuntu Server and one Windows 11 Enterprise. I assumed standard resource allocation based on my host's capacity (2 GB RAM for Linux, 4 GB for Windows) and configured them in a NAT Network so they can safely reach the internet and communicate with each other without conflicting with the campus Wi-Fi security policies.
## 3. What I did
1. **Resource Allocation:** My host machine is a Maibenben X16C with 16 GB of RAM. I allocated 2 GB RAM for the Ubuntu Server and 4 GB RAM for Windows 11. *Reason:* This leaves 10 GB for the host, ensuring the host remains stable while both VMs are running, which is critical for a safe test environment[cite: 31].
2. **Network Configuration:** I configured both VMs to use a "NAT Network" rather than the default NAT or Bridged Adapter. *Reason:* The campus Wi-Fi filters by MAC address (blocking Bridged mode), and the standard NAT does not allow guests to ping each other. NAT Network allows both VM-to-VM and VM-to-Internet connectivity.
3. **Windows Firewall Rule:** Pinging from Ubuntu to Windows initially timed out. I opened PowerShell as Administrator in Windows and ran:
   `New-NetFirewallRule -DisplayName "Allow Ping ICMPv4" -Protocol ICMPv4 -IcmpType 8 -Action Allow`
   *Reason:* Windows Defender Firewall blocks incoming ICMP (ping) requests by default. The rule is required to verify connectivity.
4. **Documentation:** I created `BUILD.md` with step-by-step instructions and screenshots so a non-technical person (like the Office Manager) can replicate the build.

## 4. Evidence
* [evidence/01-new-vm-wizard.png](evidence/01-new-vm-wizard.png) — Proves the creation step and initial naming of the VM.
* [evidence/02-disk-settings.png](evidence/02-disk-settings.png) — Shows the dynamic/thin-provisioned disk allocation to save host storage.
* [evidence/03-network-adapter.png](evidence/03-network-adapter.png) — Confirms the NAT Network selection required for campus Wi-Fi compatibility[cite: 18].
* [evidence/04-guest-uname.txt](evidence/04-guest-uname.txt) — Proves the Linux guest is successfully installed and running[cite: 18].
* [evidence/05-ping-between-guests.txt](evidence/05-ping-between-guests.txt) — Proves that both VMs can reach each other and the internet[cite: 18].
* [evidence/06-snapshot-list.png](evidence/06-snapshot-list.png) — Proves that a clean snapshot was taken after setup so the developer can safely "try things without breaking anything"[cite: 18].

## 5. What I would do differently
I initially wasted time troubleshooting a 50% packet loss when pinging the internet (8.8.8.8) from Ubuntu. It turned out to be the NAT router's initial delay processing ICMP packets. In the future, I would immediately test with a second address (like Cloudflare's 1.1.1.1) to confirm if the packet loss is persistent or just an initialization delay, rather than assuming my network configuration was wrong.
