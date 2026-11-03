# Building a Linux and a Windows test machine in Oracle VirtualBox

This document explains how to build two virtual machines (VMs) from nothing: an Ubuntu Linux VM and a Windows 11 VM. Both can reach the internet and can ping each other. Follow the steps in order.

## Before you start
You need:
* A computer with hardware virtualization enabled in the BIOS/UEFI.
* At least 16 GB of RAM and enough free disk space.
* **Oracle VirtualBox:** Go to virtualbox.org and download the latest release for your system.
* **An Ubuntu ISO:** Download from ubuntu.com/download/server (choose the current LTS release).
* **A Windows 11 ISO:** Download the Windows 11 Enterprise 64-bit edition from the official Microsoft evaluation center.
* Save both ISOs in your Downloads folder.

---

## Part A. The Ubuntu (Linux) VM

### Step 1. Start the New Virtual Machine wizard
Open VirtualBox. Click the **New** button. 
* **Name:** `test-dev-machine` (or `lab01-ubuntu`)
* **Type:** Linux
* **Version:** Ubuntu (64-bit)
* **ISO Image:** Select your downloaded Ubuntu Server ISO.
![](evidence/01-new-vm-wizard.png)

*Why:* Selecting the correct OS Type and Version ensures VirtualBox applies the safe defaults for hardware compatibility.

### Step 2. Set memory and processors
Allocate **2048 MB (2 GB)** of RAM and **2 Processors**.
*Why:* The host has 16 GB of RAM. 2 GB is plenty for a Linux server without a desktop, leaving maximum resources for the host and the heavier Windows VM.

### Step 3. Set the disk size
Set the disk size to **25 GB**. Ensure the disk type is set to **Dynamically allocated**.
![](evidence/02-disk-settings.png)

*Why 25 GB and Dynamically allocated:* Ubuntu needs around 10-15 GB. A dynamic disk (thin provisioning) only uses real physical space on your laptop as it fills up, rather than reserving all 25 GB immediately.

### Step 4. Check the network adapter
Go to the VM **Settings** > **Network**.
Make sure **Attached to:** is set to **NAT Network**.
![](evidence/03-network-adapter.png)

*Why NAT Network and not Bridged:* Bridged networking is blocked by the campus Wi-Fi MAC address filters. NAT Network puts all guests on a shared virtual network so they can reach the internet and ping each other safely.

### Step 5. Install Ubuntu
Click **Start** to boot the VM. 
* Choose **Install Ubuntu Server**. 
* Select English language and default keyboard. 
* Network: leave as default (DHCP).
* Storage: Use an entire disk (this is the 25 GB virtual disk, your laptop's real disk is safe). 
* Profile: Create a short username (e.g., `yukit0shi`) and password. 
* Tick **Install OpenSSH server**. 
* Wait for the installation to finish and choose **Reboot Now**.

### Step 6. Record the system information
Log in at the console with your new username and password. Run these commands:
`uname -a`
`lsb_release -a`

See evidence: [04-guest-uname.txt](evidence/04-guest-uname.txt)
*Check:* The output proves that the Linux kernel is running and the Ubuntu distribution is correctly installed.

---

## Part B. The Windows 11 VM

### Step 7. Start the wizard for Windows
Click **New**. 
* Name: `test-dev-windows`. 
* Type: Microsoft Windows. 
* Version: Windows 11 (64-bit). 
* Select the Windows ISO.

### Step 8. Enable TPM and EFI (CRITICAL)
Open the Windows VM **Settings** > **System**:
* On the **Motherboard** tab: Tick **Enable EFI (special OSes only)**.
* On the **Security** tab: Tick **Enable TPM 2.0**.

*Why:* Windows 11 has strict hardware requirements. It will refuse to install if a Trusted Platform Module (TPM 2.0) and Secure Boot (EFI) are not provided by the hypervisor.

### Step 9. Set hardware, network, and install
* **Memory & CPU:** 4096 MB (4 GB) RAM and 2 Processors.
* **Network:** Set to **NAT Network** (exactly the same as the Ubuntu VM).
* **Install:** Click Start. Follow the setup: select "I don't have a product key", choose Windows 11 Enterprise, and choose "Custom: Install Windows only". Create a local offline account when prompted.

---

## Part C. Network & Firewall

### Step 10. Test internet and guest-to-guest ping
Both machines need to communicate. 
See evidence: [05-ping-between-guests.txt](evidence/05-ping-between-guests.txt)

*Why ping fails from Ubuntu to Windows at first:* Windows Defender Firewall blocks incoming ICMPv4 (ping) requests by default. It causes 100% packet loss.
*How to fix it:* Open PowerShell as Administrator in Windows and run:
`New-NetFirewallRule -DisplayName "Allow Ping ICMPv4" -Protocol ICMPv4 -IcmpType 8 -Action Allow`

*Check:* Pings from Ubuntu to Windows, and from both VMs to the internet (`8.8.8.8`), now show 0% packet loss.

---

## Part D. Snapshots

### Step 11. Take a snapshot of the VMs
In the VirtualBox main window, select the VM, open the menu next to its name, and choose **Snapshots**. Click **Take**, name it `Clean installation`, and save.
![](evidence/06-snapshot-list.png)

*Why:* The office manager requested a machine where the developer can "try things without breaking anything." If the OS gets destroyed during testing, you do not need to reinstall it. Simply go to Snapshots and click **Restore** to return to this fresh, working state in seconds.
