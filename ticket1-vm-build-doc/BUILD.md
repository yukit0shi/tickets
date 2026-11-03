# VM Build Guide for Test Machines

This step-by-step guide will help you build Linux (Ubuntu) and Windows test machines from scratch.

## Step 1. Download Prerequisites
Before starting, download the following free software to your computer:
1. **Oracle VirtualBox:** Download and install from `virtualbox.org`.
2. **Ubuntu Server ISO:** Download from `ubuntu.com/download/server`.
3. **Windows 11 ISO:** Download from `microsoft.com/software-download/windows11`.

## Step 2. Creating the Virtual Machines
Open VirtualBox and click **New** to create the machines.

**For the Linux VM (Ubuntu):**
* Set Type to **Linux** and Version to **Ubuntu (64-bit)**.
![](evidence/01-new-vm-wizard.png)
*Reasoning:* Selecting the correct OS version ensures VirtualBox applies the right hardware defaults.

**For the Windows VM (Windows 11):**
* Set Type to **Microsoft Windows** and Version to **Windows 11 (64-bit)**.
* **CRITICAL:** Under *Settings -> System*, you must enable **EFI** (Motherboard tab) and enable **TPM 2.0** (Security tab).
*Reasoning:* Windows 11 will refuse to install if TPM 2.0 and Secure Boot (EFI) are not enabled inграмма virtual hardware.

## Step 3. Disk Allocation
When setting up the virtual hard disks, ensure the type is set to **Dynamically allocated**.
![](evidence/02-disk-settings.png)
*Reasoning:* A dynamically allocated disk only takes up space on your physical laptop as it fills up, rather than reserving all 25 GB instantly.

## Step 4. Network Configuration
In the VM settings, under Network Adapter, choose **NAT Network**.
![](evidence/03-network-adapter.png)
*Reasoning:* The campus Wi-Fi blocks Bridged mode. NAT Network bypasses this block, allowing the VMs to reach the internet and talk to each other safely.

## Step 5. Operating System Installation
1. Click **Start** on each VM.
2. When prompted, select the Ubuntu or Windows ISO file you downloaded in Step 1.
3. Follow the on-screen prompts to install the OS. (For Ubuntu, create a default user; for Windows, skip the product key to use the evaluation license).

## Step 6. Verifying the System
Once installed, verify the kernel and network connections.
See evidence: [04-guest-uname.txt](evidence/04-guest-uname.txt) and [05-ping-between-guests.txt](evidence/05-ping-between-guests.txt)

## Step 7. Creating a Snapshot (Restore Point)
Before handing the machine over to the developer, take a clean snapshot.
![](evidence/06-snapshot-list.png)

**How to roll back if the system breaks:**
If the developer breaks the machine, you do not need to reinstall the OS. Simply turn off the VM, go to the **Snapshots** menu, select `Clean installation`, and click **Restore**.
*Reasoning:* This fulfills the requirement to let the developer "try things without breaking anything."
