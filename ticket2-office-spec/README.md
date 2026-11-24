# Ticket 2 — Kitting out the new office

## 1. What was asked

> From: Daniyar K. (Director)
> Subject: computers for the new office
>
> We are opening the second office and need four computers — accountant, sales, web developer, designer. Budget 3,000,000 tenge for everything. Everyone says we should just buy the same four machines, is that right? Make sure they last a few years. Monitors included.

## 2. What was actually needed

The director's idea of buying four identical machines is wrong. Each role runs a completely different daily workload, which means each workstation hits a different hardware bottleneck:

- **Accountant:** Runs local accounting software (1C:Enterprise), tax portals, and spreadsheets. This requires native Windows compatibility and decent single-core responsiveness, not a dedicated graphics card or 32 GB of RAM.
- **Sales Manager:** Keeps 35–40 browser tabs, CRM systems, and video calls running at the same time. Here, **RAM capacity** is the main bottleneck—if memory fills up, the OS starts swapping to disk during client calls.
- **Web Developer:** Runs local servers, Docker containers/VMs, IDEs, databases, and multiple browser windows simultaneously, requiring **multi-core CPU performance and 32 GB RAM**.
- **Designer:** Works with multi-layer graphics and high-resolution canvases, where **GPU acceleration (VRAM) and a sharp, larger QHD IPS display** matter most.

Splitting the **3,000,000 KZT** budget equally across four identical PCs would waste money on unneeded GPUs for office roles while leaving the designer and developer underpowered.

**What the request left out:**
1. **Exact lifespan:** "A few years" is vague—planning for 3 years vs. 5+ years changes platform and upgrade decisions.
2. **Peripherals and accessories:** Keyboards, mice, Ethernet/display cables, headsets/webcams for video calls, and surge protectors or UPS units were not mentioned.
3. **Specific software stack:** Which exact tools each person uses (such as 1C:Enterprise and banking tokens on Windows vs. Linux tooling for development, or 2D vs. 3D/video editing for design).
4. **Workspace constraints:** Case size limits, Wi-Fi vs. wired Ethernet availability, and whether the company prefers custom builds or pre-built warranty coverage.

**What I would ask Daniyar:**
- How many years do you expect these machines to last before replacement, and do you plan to upgrade RAM or storage mid-cycle?
- What specific applications will each person run daily (especially whether the designer handles 3D/video rendering and whether accounting requires Windows-only software)?
- Are keyboards, mice, headsets, and network cables covered by a separate office supplies budget, or must they fit inside the 3,000,000 KZT limit?

**What I assumed instead:**
- Targeted a **3–4 year service life** using upgradeable Micro-ATX desktop platforms.
- Assumed keyboards, mice, and patch cables are funded separately under general office supplies, while **all four monitors and OS licenses** are included within the 3,000,000 KZT budget.
- Chose **Windows 11 Pro** for the Accountant, Sales Manager, and Designer (for full compatibility with local accounting tools and commercial design software) and **Ubuntu 26.04.1 LTS** for the Web Developer.

## 3. What I did

I started from each role's daily workload, identified which resource runs out first, and directed the budget toward those bottlenecks while meeting every hard requirement ($\ge$ 16 GB RAM, 1 TB NVMe SSD, licensed OS, and one monitor per workstation). Part compatibility and wattage margins were verified on PCPartPicker, and local Kazakhstan prices with dates (`2026-11-22`) were recorded in [`budget.csv`](budget.csv) with build trade-offs documented in [`builds.md`](builds.md).

The total cost across all four workstations is **2,921,408 KZT**, leaving **78,592 KZT** under the 3,000,000 KZT budget.

**1. Which of the four machines will you upgrade first, eighteen months from now, and why?**
I would upgrade the **Accountant's** workstation first by replacing or expanding its **RAM** from 16 GB to 32 GB. It is the only build starting at the 16 GB baseline, and as 1C databases, multi-year Excel ledgers, browser portals, and background security tools grow heavier over 18 months, 16 GB will tighten first while the other three machines already ship with 32 GB.

**2. Price the same four as pre-built machines from a vendor. What does the company gain and what does it lose by not building them?**
Checking comparable pre-built desktops plus monitors from Kazakhstan retail listings on `2026-11-22` comes out slightly higher than our custom total while offering worse component balance:

| Role | Pre-built Desktop + Monitor Equivalent | Vendor | Price (KZT) | Date |
| :--- | :--- | :--- | ---: | :--- |
| Accountant | Pre-built Core i3-12100 / 16GB / 1TB SSD + 24" IPS Monitor | Kaspi.kz / DNS | 425,000 | 2026-11-22 |
| Sales | Pre-built Ryzen 5 5600G / 32GB / 1TB SSD + 24" IPS Monitor | Kaspi.kz / DNS | 685,000 | 2026-11-22 |
| Web Developer | Pre-built Core i5-12400 / 32GB / 1TB SSD + 24" IPS Monitor | Kaspi.kz / DNS | 665,000 | 2026-11-22 |
| Designer | Pre-built Core i5-12400 / 32GB / 1TB / RTX 3060 + 27" QHD Monitor | Kaspi.kz / DNS | 1,190,000 | 2026-11-22 |
| **Pre-built Total** | — | — | **2,965,000** | **2026-11-22** |

By buying pre-built systems, the company **gains** a single unified vendor warranty, zero assembly time, and immediate deployment out of the box. However, it **loses** control over internal component quality (pre-builts often use entry-level PSUs, slower SSDs, or single-channel memory), pays for bundled Windows licenses even on the Linux developer machine, and faces stricter upgrade paths.

**3. One of these four people will complain about performance within a month. Which one, and about what?**
The **Designer** is the most likely to complain within the first month if their workload turns out to include heavy 4K video editing, After Effects compositions, or 3D rendering rather than 2D graphic/UI design. To keep the overall budget under 3,000,000 KZT, I capped the Designer's CPU at a 6-core Intel Core i5-12400 and 32 GB of DDR4 RAM instead of an 8+ core processor with 64 GB RAM, which will noticeably slow down long timeline exports and heavy multi-layer renders.

## 4. Evidence

- [`evidence/pcpartpicker-links.md`](evidence/pcpartpicker-links.md) — Contains the four PCPartPicker build links confirming socket, DDR4 memory, M.2 slot, GPU clearance, and PSU wattage compatibility.
- [`evidence/vendor-quotes/`](evidence/vendor-quotes/) — Dated vendor screenshots (`2026-11-22`) proving every component, monitor, and OS price listed in [`budget.csv`](budget.csv):
  - **Accountant:** `accountant-cpu.png`, `accountant-cooler.png`, `accountant-motherboard.png`, `accountant-memory.png`, `accountant-storage.png`, `accountant-psu.png`, `accountant-case.png`, `accountant-monitor.png`, `accountant-os.png`.
  - **Sales:** `sales-cpu.png`, `sales-cooler.png`, `sales-motherboard.png`, `sales-memory.png`, `sales-storage.png`, `sales-psu.png`, `sales-case.png`, `sales-monitor.png`, `sales-os.png`.
  - **Web Developer:** `web-developer-cpu.png`, `web-developer-cooler.png`, `web-developer-motherboard.png`, `web-developer-memory.png`, `web-developer-storage.png`, `web-developer-psu.png`, `web-developer-case.png`, `web-developer-monitor.png`, `web-developer-os.png`.
  - **Designer:** `designer-cpu.png`, `designer-cooler.png`, `designer-motherboard.png`, `designer-memory.png`, `designer-storage.png`, `designer-gpu.png`, `designer-psu.png`, `designer-case.png`, `designer-monitor.png`, `designer-os.png`.

## 5. What I would do differently

Before buying parts, I would confirm the exact software list and monitor color-calibration requirements with the Designer and Web Developer, and I would standardize on identical DDR4 motherboards across the office machines so spare RAM sticks or replacement parts can be swapped between workstations if one fails.
