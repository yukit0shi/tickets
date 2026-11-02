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
Instead of delaying the setup to ask for clarification, I assumed the developer might need to test cross-platform environments. Therefore, I built two guest virtual machines: one Ubuntu Server and one Windows 11 Enterprise[cite: 31]. I assumed standard resource allocation based on my host's capacity (2 GB RAM for Linux, 4 GB for Windows) and configured them in a NAT Network so they can safely reach the internet and communicate with each other without conflicting with the campus Wi-Fi security policies.
