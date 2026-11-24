# Workstation Builds & Trade-off Analysis

Grand total across all four workstations: **2,921,408 KZT** (within the **3,000,000 KZT** budget, leaving **78,592 KZT** unspent). Every workstation meets the baseline requirements: ≥ 16 GB RAM, 1 TB NVMe SSD, a licensed OS, and one monitor.

---

### Build 1 — Accountant

- **Load:** 1C:Enterprise accounting databases, tax and banking web portals, multi-sheet Excel workbooks, PDF invoices, and email over an 8-hour workday.
- **Bottleneck:** Single-core CPU responsiveness for spreadsheet recalculations, storage reliability/endurance (600 TBW on the Samsung 990 PRO) for local accounting databases, and native Windows OS compatibility—rather than GPU compute or 32 GB RAM.
- **Trade-off:** Selected a 4-core Intel Core i3-12100 with integrated UHD 730 graphics and a **16 GB (2×8 GB) DDR4** kit on a 2-slot H610 board instead of stepping up to a 6-core Core i5-12400 and 32 GB RAM (saving ~197,000 KZT), redirecting that budget to memory- and GPU-heavy workstations.
- **Total:** **551,247 KZT** (`budget.csv`, rows 2–10)

---

### Build 2 — Sales Manager

- **Load:** Cloud CRM with 35–40 active browser tabs, Excel price lists, messengers, and 3–4 hours of daily video calls running concurrently.
- **Bottleneck:** **Memory capacity (RAM)**. Heavy browser tab counts combined with video conferencing consume 12–15 GB; on an 8–16 GB machine, the OS starts swapping to disk mid-call and degrades audio/video responsiveness.
- **Trade-off:** Chose an AMD Ryzen 5 5600G on an end-of-life AM4/DDR4 board (`MSI A520M-A PRO`, noting that the motherboard requires a BIOS revision supporting Ryzen 5000G APUs) with integrated Vega graphics instead of a newer AM5/DDR5 platform or discrete GPU, putting the saved budget into **32 GB DDR4** and a compact mesh chassis so all CRM tabs and video calls stay in RAM.
- **Total:** **772,619 KZT** (`budget.csv`, rows 12–20)

---

### Build 3 — Web Developer

- **Load:** Local development servers, Docker containers/VMs, code editors/IDEs, local databases, and multi-tab browser testing across an 8-hour day.
- **Bottleneck:** **Multi-core CPU throughput and RAM capacity** when running concurrent local services, containers, databases, and browser environments.
- **Trade-off:** Used **Ubuntu 26.04.1 LTS** (legally licensed under the open-source GNU GPL at 0 KZT, saving 46,885 KZT vs. Windows 11 Pro) and integrated graphics instead of a discrete GPU, redirecting those funds into a 6-core/12-thread Intel Core i5-12400 and **32 GB DDR4**.
- **Total:** **565,479 KZT** (`budget.csv`, rows 22–30)

---

### Build 4 — Designer

- **Load:** Visual design software (Figma, Photoshop, Illustrator), multi-layer assets, high-resolution exports, and layout work over an 8-hour workday.
- **Bottleneck:** **Display workspace/color reproduction (27" QHD IPS)** paired with **GPU acceleration/VRAM** for canvas rendering and 32 GB RAM for large multi-layer files.
- **Trade-off:** Allocated budget to a dedicated **GIGABYTE GeForce RTX 3060 12 GB**, **32 GB DDR4**, and a **600W PSU**, while keeping the processor at a 6-core Intel Core i5-12400 and choosing an uncalibrated office 27" QHD IPS panel (`Dell Pro E2726DS`, 99% sRGB) instead of a factory-calibrated wide-gamut design monitor (saving ~100,000+ KZT).
- **Total:** **1,032,063 KZT** (`budget.csv`, rows 32–41)
