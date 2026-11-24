# Ticket 2 — Kitting out the new office

## 1. What was asked

> From: Daniyar K. (Director)
> Subject: computers for the new office
>
> We are opening the second office and need four computers — accountant, sales, web developer, designer. Budget 3,000,000 tenge for everything. Everyone says we should just buy the same four machines, is that right? Make sure they last a few years. Monitors included.

## 2. What was actually needed

Buying four identical machines is the wrong approach. Each role runs a distinct daily workload, shifting the hardware bottleneck to a different subsystem:
- **Accountant:** Runs local accounting software (1C:Enterprise), tax portals, and spreadsheets. This requires native Windows compatibility and single-core responsiveness, not a discrete GPU or 32 GB RAM.
- **Sales Manager:** Keeps 35–40 browser tabs, CRM systems, and video calls open simultaneously, making **RAM capacity** the main bottleneck so the OS avoids swapping during client calls.
- **Web Developer:** Runs local servers, Docker containers/VMs, IDEs, and databases concurrently, requiring **multi-core CPU throughput and 32 GB RAM**.
- **Designer:** Works with multi-layer graphics and high-resolution canvases, where **display resolution/color accuracy and GPU acceleration (VRAM)** matter most.

Splitting the **3,000,000 KZT** budget equally across four identical PCs would waste money on unused GPUs for office roles while leaving the designer and developer under-equipped.

**What the request left out:**
1. **Exact service lifespan:** "A few years" is ambiguous—planning for 3 years vs. 5+ years changes platform and upgrade decisions.
2. **Peripherals and power protection:** Keyboards, mice, Ethernet/display cables, webcams/headsets for sales calls, and surge protectors or UPS units (especially critical for the accountant) were not mentioned.
3. **Specific software stack:** Which exact tools each person runs daily (such as 1C:Enterprise and banking tokens on Windows vs. Linux tooling for development, or 2D UI vs. print/3D/video design).
4. **Workspace constraints:** Desk space/case form factor limits and whether Wi-Fi adapters or wired Ethernet drops are available.

**Questions I would ask Daniyar:**
- How many years should these machines serve before replacement, and do you expect to upgrade RAM or CPUs mid-cycle?
- Which exact applications will each role use daily (specifically whether the designer needs factory color calibration for print/brand work or 3D/video rendering)?
- Are keyboards, mice, headsets, patch cables, and UPS units covered by a separate office supplies budget, or must they fit inside the 3,000,000 KZT limit?

**Assumptions I made instead:**
- Targeted a **3–4 year service life** without requiring mid-cycle motherboard/CPU platform swaps (accepting that the Sales build uses the mature AM4 platform for cost efficiency over this 3–4 year window).
- Assumed keyboards, mice, cables, and UPS units are funded separately under general office supplies, while **all four monitors and OS licenses** are included inside the 3,000,000 KZT budget.
- Chose **Windows 11 Pro** for the Accountant, Sales Manager, and Designer (for full compatibility with Kazakhstan accounting/tax tools and commercial creative suites) and **Ubuntu 26.04.1 LTS** for the Web Developer. Ubuntu is legally licensed under open-source terms (**GNU GPL**, 0 KZT per seat with 5 years of LTS security support) and is natively **suitable** for web development (native Docker Engine without WSL overhead, Linux CLI/SSH parity with production servers, and local databases).

## 3. What I did

I mapped each role's daily workload to its primary bottleneck and allocated the budget toward the parts that prevent slowdowns under load while meeting every hard rule (≥ 16 GB RAM, 1 TB NVMe SSD, licensed OS, and one monitor per desk). Hardware compatibility and wattage margins were checked on PCPartPicker (noting the standard BIOS revision check for the Ryzen 5 5600G on the MSI A520M-A PRO board), and local KZT prices were logged in [`budget.csv`](budget.csv) alongside build justifications in [`builds.md`](builds.md).

Total spend across all four workstations is **2,921,408 KZT**, leaving **78,592 KZT** under the 3,000,000 KZT budget.

**1. Which of the four machines will you upgrade first, eighteen months from now, and why?**
I would upgrade the **Accountant's** machine first by **replacing its 16 GB (2×8 GB) DDR4 kit with a 32 GB (2×16 GB) DDR4 kit**. Because the `GIGABYTE H610M K DDR4` motherboard only has two DIMM slots (both populated by the 2×8 GB sticks), memory cannot simply be expanded by adding a stick—the kit must be swapped. As 1C databases, multi-year Excel ledgers, browser portals, and background security tools grow heavier over 18 months, 16 GB will tighten first while the other three systems already start with 32 GB.

**2. Price the same four as pre-built machines from a vendor. What does the company gain and what does it lose by not building them?**
Comparable pre-built desktops plus monitors from Kazakhstan retail listings (checked on `2026-11-24`) cost roughly the same or slightly more than our custom total while offering worse component balance:

| Role | Pre-built Desktop + Monitor Equivalent | Source URL | Price (KZT) | Date |
| :--- | :--- | :--- | ---: | :--- |
| Accountant | [Модель готового ПК на i3-12100/16GB/1TB + монитор 24"] | [ВСТАВЬ_ССЫЛКУ_1] | [ЦЕНА_1] | 2026-11-24 |
| Sales | [Модель готового ПК на Ryzen 5 5600G/32GB/1TB + монитор 24"] | [ВСТАВЬ_ССЫЛКУ_2] | [ЦЕНА_2] | 2026-11-24 |
| Web Developer | [Модель готового ПК на i5-12400/32GB/1TB + монитор 24"] | [ВСТАВЬ_ССЫЛКУ_3] | [ЦЕНА_3] | 2026-11-24 |
| Designer | [Модель готового ПК на i5-12400/32GB/1TB/RTX 3060 + 27" QHD] | [ВСТАВЬ_ССЫЛКУ_4] | [ЦЕНА_4] | 2026-11-24 |
| **Pre-built Total** | — | — | **[СУММА]** | **2026-11-24** |

By choosing pre-built systems, the company **gains** a single unified vendor warranty, zero assembly labor, and immediate out-of-the-box deployment. However, it **loses** transparency and control over internal component models (such as PSU quality and SSD endurance), pays for bundled Windows licenses even on the Linux developer workstation, and faces stricter per-part upgrade paths.

**3. One of these four people will complain about performance within a month. Which one, and about what?**
The **Designer** is the most likely to complain within the first month—specifically about **display color accuracy** if their tasks involve print layouts or brand-critical color grading. To afford the RTX 3060 12 GB and 32 GB RAM within budget, I selected the `Dell Pro 27 QHD Monitor E2726DS`, which is a standard office IPS panel (99% sRGB) without factory Delta-E calibration or wide DCI-P3/AdobeRGB gamut coverage (and secondarily, the 6-core i5-12400 / 32 GB RAM will feel tight if they run heavy 3D or 4K video renders).

## 4. Evidence

- [`evidence/pcpartpicker-links.md`](evidence/pcpartpicker-links.md) — Lists the four PCPartPicker build links verifying socket, DDR4, M.2, case clearance, and PSU wattage compatibility.
- [`evidence/vendor-quotes/`](evidence/vendor-quotes/) — Dated vendor screenshots (`2026-11-22`) proving all 37 component, monitor, and OS prices recorded in [`budget.csv`](budget.csv):
  - **Accountant:** `accountant-cpu.png`, `accountant-cooler.png`, `accountant-motherboard.png`, `accountant-memory.png`, `accountant-storage.png`, `accountant-psu.png`, `accountant-case.png`, `accountant-monitor.png`, `accountant-os.png`.
  - **Sales:** `sales-cpu.png`, `sales-cooler.png`, `sales-motherboard.png`, `sales-memory.png`, `sales-storage.png`, `sales-psu.png`, `sales-case.png`, `sales-monitor.png`, `sales-os.png`.
  - **Web Developer:** `web-developer-cpu.png`, `web-developer-cooler.png`, `web-developer-motherboard.png`, `web-developer-memory.png`, `web-developer-storage.png`, `web-developer-psu.png`, `web-developer-case.png`, `web-developer-monitor.png`, `web-developer-os.png`.
  - **Designer:** `designer-cpu.png`, `designer-cooler.png`, `designer-motherboard.png`, `designer-memory.png`, `designer-storage.png`, `designer-gpu.png`, `designer-psu.png`, `designer-case.png`, `designer-monitor.png`, `designer-os.png`.

## 5. What I would do differently

1. **Rebalance SSD and case spending to fund a calibrated Designer display and peripherals:** Spending 141,150 KZT each on PCIe 4.0 `Samsung 990 PRO` drives and 69,999 KZT each on `Lian Li A3-mATX` cases for the Accountant and Sales machines was overkill for office workloads. Replacing those with `ADATA LEGEND 710` SSDs (60,870 KZT) and `Deepcool MATREXX 30` cases (13,960 KZT) would save **272,638 KZT**, enough to upgrade the Designer to a factory-calibrated monitor (such as an ASUS ProArt or Dell UltraSharp) and include keyboards, mice, and a UPS within the 3,000,000 KZT cap.
2. **Verify BIOS support or move Sales off AM4:** Before ordering the `MSI A520M-A PRO` + `Ryzen 5 5600G`, I would confirm with the seller that the shipped BIOS revision supports Cezanne APUs out of the box—or switch the Sales build to a B550/AM5 platform for cleaner long-term upgradability.
