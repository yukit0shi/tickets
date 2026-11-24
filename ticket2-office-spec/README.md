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
I would upgrade the **Accountant's** machine first by **replacing its 16 GB (2×8 GB) DDR4 kit with a 32 GB (2×16 GB) DDR4 kit**. Because the `GIGABYTE H610M K DDR4` motherboard only has two DIMM slots (both populated by the 2×8 GB sticks), memory cannot simply be expanded by adding a stick—a full 2×16 GB kit must be purchased while the original 2×8 GB kit (114,000 KZT) moves to spare inventory. As 1C databases, multi-year Excel ledgers, browser portals, and background security tools grow heavier over 18 months, 16 GB will tighten first while the other three systems already start with 32 GB.

**2. Price the same four as pre-built machines from a vendor. What does the company gain and what does it lose by not building them?**
Comparable pre-built desktops plus monitors from Kaspi.kz (checked on `2026-11-24`) total **2,397,468 KZT**, which is **523,940 KZT (~21.9%) less** than our custom build total (`2,921,408 KZT`):

| Role | Pre-built Desktop + Monitor Equivalent | Source URL | Price (KZT) | Date |
| :--- | :--- | :--- | ---: | :--- |
| Accountant | Комплект ПК VTX SET-OFF-368 (Core i3-13100 / 16GB / 1TB SSD + 2TB HDD / Win 11 Pro + Monitor) | https://kaspi.kz/shop/p/vtx-set-off-368-chernyi-178179246/ | 403,300 | 2026-11-24 |
| Sales | Системный блок Tehnicom (Ryzen 5 5600G / 32GB / 4TB SSD + 4TB HDD / Win 11) + Xiaomi A24i (45,990 KZT) | https://kaspi.kz/shop/p/tehnicom-art52530-belyi-171146976/ | 783,828 | 2026-11-24 |
| Web Developer | Системный блок Ucomp (Core i5-12400 / 32GB / 1TB SSD + 2TB HDD / OS not listed; Ubuntu 0 KZT) + Xiaomi A24i (45,990 KZT) | https://kaspi.kz/shop/p/ucomp-ucomp-i5-12400-uhd7301gb-32ddr4-1t-hdd2tb-h610-600w-dk151-air-black-chernyi-149065416/ | 557,171 | 2026-11-24 |
| Designer | Игровой системный блок Tehnicom (Core i5-12400F no iGPU / RTX 3060 12GB / 32GB / 1TB SSD / Win 10) + Dell E2726DS (103,169 KZT) | https://kaspi.kz/shop/p/tehnicom-i5-12400f-rtx-3060-32gb-1000-ssd-win-10-109358073/ | 653,169 | 2026-11-24 |
| **Pre-built Total** | — | — | **2,397,468** | **2026-11-24** |

By choosing pre-built systems, the company **gains** ~523,940 KZT in upfront savings, a single unified seller warranty, zero assembly labor, and immediate out-of-the-box deployment. However, it **loses** control over internal component models (prebuilt listings omit exact PSU, motherboard, and NVMe endurance ratings), receives an older OS and F-series CPU without integrated backup graphics on the Designer unit (`Windows 10` + `i5-12400F` vs. `Windows 11 Pro` + `i5-12400`), and faces stricter per-part upgrade paths.

**Was building custom worth the ~21.9% (523,940 KZT) premium?** Breaking the comparison down by role shows that custom building is actually **cheaper or nearly price-neutral** for the Sales Manager (`772,619` vs. `783,828 KZT`, saving `11,209 KZT`) and Web Developer (`565,479` vs. `557,171 KZT`, `+8,308 KZT`), even with retail `32 GB DDR4` kits. Almost all of the total price gap comes from two machines: the **Designer** (`1,032,063` vs. `653,169 KZT`, `+378,894 KZT` or **72.3%** of the total difference, where the pre-built cuts costs via an `i5-12400F` without integrated graphics, `Windows 10`, and budget components around the RTX 3060—adding a `Windows 11 Pro` license at `46,885 KZT` narrows that role gap to `332,009 KZT` and the overall gap to `477,055 KZT`) and the **Accountant** (`551,247` vs. `403,300 KZT`, `+147,947 KZT` due to the `Lian Li` case, `Samsung 990 PRO`, and `114,000 KZT` 2×8 GB RAM kit). While a strict cost-cutting office could buy the pre-built machines to save ~524,000 KZT, building custom provides explicitly specified `Deepcool PF450/PF600` power supplies and SSD models with documented endurance ratings (such as the 600 TBW `Samsung 990 PRO` for 1C database writes), consistent `Windows 11 Pro` licensing across all Windows desks, and clean component-level maintainability over a 3–4 year lifespan.

**3. One of these four people will complain about performance within a month. Which one, and about what?**
The **Designer** is the most likely to complain within the first month—specifically about **display color accuracy** if their tasks involve print layouts or brand-critical color grading. To afford the RTX 3060 12 GB and 32 GB RAM within budget, I selected the `Dell Pro 27 QHD Monitor E2726DS`, which is a standard office IPS panel without factory Delta-E calibration or wide DCI-P3/AdobeRGB gamut coverage (and secondarily, the 6-core i5-12400 / 32 GB RAM will feel tight if they run heavy 3D or 4K video renders).

## 4. Evidence

- [`evidence/pcpartpicker-links.md`](evidence/pcpartpicker-links.md) — Lists the four PCPartPicker build links verifying socket, DDR4, M.2, case clearance, and PSU wattage compatibility.
- [`evidence/vendor-quotes/`](evidence/vendor-quotes/) — Dated vendor screenshots (`2026-11-22`) proving all 37 component, monitor, and OS prices recorded in [`budget.csv`](budget.csv):
  - **Accountant:** `accountant-cpu.png`, `accountant-cooler.png`, `accountant-motherboard.png`, `accountant-memory.png`, `accountant-storage.png`, `accountant-psu.png`, `accountant-case.png`, `accountant-monitor.png`, `accountant-os.png`.
  - **Sales:** `sales-cpu.png`, `sales-cooler.png`, `sales-motherboard.png`, `sales-memory.png`, `sales-storage.png`, `sales-psu.png`, `sales-case.png`, `sales-monitor.png`, `sales-os.png`.
  - **Web Developer:** `web-developer-cpu.png`, `web-developer-cooler.png`, `web-developer-motherboard.png`, `web-developer-memory.png`, `web-developer-storage.png`, `web-developer-psu.png`, `web-developer-case.png`, `web-developer-monitor.png`, `web-developer-os.png`.
  - **Designer:** `designer-cpu.png`, `designer-cooler.png`, `designer-motherboard.png`, `designer-memory.png`, `designer-storage.png`, `designer-gpu.png`, `designer-psu.png`, `designer-case.png`, `designer-monitor.png`, `designer-os.png`.
- **Pre-built vendor comparison (`2026-11-24`):** Documented via direct dated Kaspi.kz product URLs in Section 3 (Question 2).

## 5. What I would do differently

1. **Rebalance Sales SSD and office case spending to fund a calibrated Designer display and peripherals:** While keeping the high-endurance `Samsung 990 PRO` (600 TBW) for the Accountant's local 1C database writes, using a PCIe 4.0 `Samsung 990 PRO` (141,150 KZT) for the Sales Manager and `Lian Li A3-mATX` cases (69,999 KZT each) for both Accountant and Sales was unnecessary for office workloads. Replacing the Sales SSD with an `ADATA LEGEND 710` (60,870 KZT) and both cases with `Deepcool MATREXX 30` chassis (13,960 KZT each) would save **192,358 KZT**—enough to upgrade the Designer to a factory-calibrated monitor (such as an ASUS ProArt or Dell UltraSharp) and include keyboards, mice, and a UPS within the 3,000,000 KZT cap.
2. **Verify BIOS support or move Sales off AM4:** Before ordering the `MSI A520M-A PRO` + `Ryzen 5 5600G`, I would confirm with the seller that the shipped BIOS revision supports Cezanne APUs out of the box—or switch the Sales build to a B550/AM5 platform for cleaner long-term upgradability.
