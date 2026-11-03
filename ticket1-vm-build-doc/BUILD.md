# Building a Linux and a Windows test machine in Oracle VirtualBox

This document explains how to build two virtual machines (VMs) from nothing: an Ubuntu Linux VM and a Windows 11 VM. Both can reach the internet and ping each other safely. 

This guide is designed for non-IT personnel. Follow the steps in strict order.

---

## Before you start (Prerequisites)
You need to download the required software and operating system images (ISOs) to your laptop's `Downloads` folder. All of these are free:

1. **Oracle VirtualBox (Hypervisor):** Go to `virtualbox.org/wiki/Downloads`, click "Windows hosts", download the installer, and run it by clicking "Next" on all default settings.
2. **Ubuntu Linux ISO:** Go to `ubuntu.com/download/server` and download the latest "Ubuntu Server LTS" version (approx. 2 GB).
3. **Windows 11 ISO:** Go to `microsoft.com/software-download/windows11`, scroll down to "Download Windows 11 Disk Image (ISO)", choose multi-edition, select English, and download (approx. 6 GB).

---

## Part A. The Ubuntu (Linux) VM

### Step 1. Start the New Virtual Machine wizard
Open Oracle VirtualBox. Click the blue **New** button at the top. 
* **Name:** Type `test-dev-machine` (or `lab01-ubuntu`).
* **ISO Image:** Click the dropdown, select "Other...", and find the Ubuntu ISO you downloaded.
* **Type:** Linux.
* **Version:** Ubuntu (64-bit).

![](evidence/01-new-vm-wizard.png)

> **Why Linux and Ubuntu (64-bit):** VirtualBox uses this selection to automatically configure the correct virtual motherboard, CPU architecture, and memory limits for the guest operating system.

### Step 2. Allocate RAM and CPU (Hardware)
Click **Next** to reach the Hardware section.
* **Base Memory:** Set to `2048 MB` (2 GB).
* **Processors:** Set to `2` CPUs.

> **Why 2048 MB:** The host laptop has 16 GB of RAM. Allocating only 2 GB to Linux leaves plenty of memory for the host system and the Windows VM, preventing the host computer from crashing.

### Step 3. Set the Disk Size
Click **Next** to reach the Virtual Hard disk section.
* Select **Create a Virtual Hard Disk Now**.
* Set the size to `25.00 GB`.
* Ensure "Pre-allocate Full Size" is **NOT** checked. Click **Finish**.

To verify the disk was created correctly, click on your new VM, go to **Settings > Storage**, and click on the `.vdi` file. Look at the information panel on the right.
![](evidence/02-disk-settings.png)

> **Why dynamically allocated (Not pre-allocated):** A dynamic disk only takes up physical space on your laptop's SSD as it fills up. Even though we set the limit to 25 GB, a fresh Ubuntu install only actually consumes about 5 GB of your laptop's real storage.

### Step 4. Check the Network Adapter
While still in **Settings**, click on the **Network** tab on the left.
* Change "Attached to" from *NAT* to **NAT Network**.
* Ensure the name says `NatNetwork`. Click **OK**.

![](evidence/03-network-adapter.png)

> **Why NAT Network and not Bridged:** The campus Wi-Fi security system blocks "Bridged" adapters because it doesn't recognize the VM's generated MAC address. "NAT Network" bypasses this block by hiding the VM behind your host laptop's IP address, while still allowing the VMs to talk to each other.

### Step 5. Install Ubuntu
Click the green **Start** arrow to turn on the VM. 
* **Boot menu:** Press Enter on "Try or Install Ubuntu Server".
* **Language:** Choose English.
* **Keyboard:** Accept default (English US).
* **Network & Mirror:** Keep pressing "Done" to accept the automatic DHCP address and default mirrors.
* **Storage:** Select "Use an entire disk" and continue. (Don't worry, this only formats the virtual 25GB disk, your host computer is safe).
* **Profile:** Set your name, server name (`lab01-ubuntu`), and username (`yukit0shi`). Pick a simple password.
* **SSH:** Check the box to "Install OpenSSH server", then select Done.
* **Finish:** Wait 5-10 minutes until you see "Reboot Now" at the bottom. Press Enter. 

### Step 6. Record the system information
Once the system restarts, log in with the username and password you created (the password will be invisible as
