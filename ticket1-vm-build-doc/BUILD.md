# VM Build Guide for Test Machines

This guide will help you build the required Linux (Ubuntu) and Windows test machines from scratch.

## Step 1. Creating the Virtual Machine
Create a new machine in VirtualBox and select the OS version. 
![](evidence/01-new-vm-wizard.png)
**Reasoning:** We select "Linux" and "Ubuntu (64-bit)" because it matches the downloaded ISO image. Building both a Linux and a Windows machine ensures the new developer has a complete cross-platform environment to test their code.

## Step 2. Disk Allocation
When setting up the virtual hard disk, ensure the type is set to dynamically allocated.
![](evidence/02-disk-settings.png)
**Reasoning:** A dynamically allocated disk only uses space on your physical hard drive as it fills up, rather than reserving the entire 25 GB at once. This prevents the host computer from running out of storage space unnecessarily.

## Step 3. Network Configuration
In the VM settings, under Network Adapter, choose **NAT Network**.
![](evidence/03-network-adapter.png)
**Reasoning:** Why NAT Network and not Bridged? The campus network filters connections by MAC address, which blocks Bridged adapters. NAT Network bypasses this restriction by hiding behind the host's IP, while still allowing both virtual machines to communicate with each other and reach the internet.

## Step 4. Verifying the Linux Guest
Start the Linux machine and run `uname -a` to verify the system kernel.
See evidence: [04-guest-uname.txt](evidence/04-guest-uname.txt)
**Reasoning:** This step confirms that the operating system is fully installed, booted, and responding to basic system commands without kernel panics.

## Step 5. Verifying Network Connectivity
Use the `ping` command to test connections from both machines.
See evidence: [05-ping-between-guests.txt](evidence/05-ping-between-guests.txt)
**Reasoning:** We must prove that the developer can reach the outside internet to download packages, and that the Windows and Linux machines can talk to each other to test client-server applications.

## Step 6. Creating a Safe Restore Point
Before handing the machine over to the developer, take a clean snapshot.
![](evidence/06-snapshot-list.png)
**Reasoning:** The request asked for a machine where the developer can "try things without breaking anything". A snapshot creates a save state. If the developer completely breaks the system, you can restore it to this clean state in seconds without rebuilding it from scratch.
