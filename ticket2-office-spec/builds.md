# Workstation Builds & Trade-off Analysis

Grand total across all four workstations: **2,921,408 KZT** (within the **3,000,000 KZT** budget, leaving **78,592 KZT** unspent). Every workstation meets the baseline requirements: $\ge$ 16 GB RAM, 1 TB NVMe SSD, a licensed OS, and one monitor.

---

### Build 1 — Accountant

- **Load:** 1C:Enterprise accounting databases, tax and banking web portals, multi-sheet Excel workbooks, PDF invoices, and email over an 8-hour workday.
- **Bottleneck:** Single-core CPU responsiveness for spreadsheet recalculations and native Windows OS compatibility, rather than GPU compute or high-capacity RAM.
- **Trade-off:** Selected a 4-core Intel Core i3-12100 with integrated UHD 730 graphics and **16 GB DDR4** instead of stepping up to a 6-core Core i5-12400 and 32 GB RAM kit (saving ~197,000 KZT), redirecting that budget to memory- and GPU-heavy workstations.
- **Total:** **551,247 KZT** (`budget.csv`, rows 2–10)

---

### Build 2 — Sales Manager

- **Load:** Cloud CRM with 35–40 active browser tabs, Excel price lists, messengers, and 3–4 hours of daily video calls running concurrently.
- **Bottleneck:** **Memory capacity (RAM)**. Heavy browser tab counts combined with video conferencing consume 12–15 GB; on an 8–16 GB machine, the OS starts swapping to disk mid-call and degrades audio/video responsiveness.
- **Trade-off:** Chose an AMD Ryzen 5 5600G on an AM4/DDR4 board with integrated Vega graphics instead of adding a discrete GPU (saving ~234,000 KZT), spending the budget on **32 GB DDR4** so all CRM tabs and video calls stay resident in RAM.
- **Total:** **772,619 KZT** (`budget.csv`, rows 12–20)

---

### Build 3 — Web Developer

- **Load:** Local development servers, Docker containers/VMs, code editors/IDEs, local databases, and multi-tab browser testing across an 8-hour day.
- **Bottleneck:** **Multi-core CPU throughput and RAM capacity** when running concurrent local services, containers, databases, and browser environments.
- **Trade-off:** Used **Ubuntu 26.04.1 LTS** (0 KZT open-source LTS license, saving 46,885 KZT vs. Windows 11 Pro) and integrated graphics instead of a discrete GPU, putting the funds into a 6-core/12-thread Intel Core i5-12400 and **32 GB DDR4**.
- **Total:** **565,479 KZT** (`budget.csv`, rows 22–30)

---

### Build 4 — Designer

- **Load:** Visual design software, multi-layer assets, high-resolution exports, and display-intensive layout work over an 8-hour workday.
- **Bottleneck:** **GPU acceleration / VRAM** for canvas rendering and hardware effects, paired with **display resolution/panel quality** (27" QHD IPS) and 32 GB RAM.
- **Trade-off:** Kept the processor at a 6-core Intel Core i5-12400 on DDR4 rather than upgrading to an 8-core Core i7 / DDR5 platform (saving ~120,000+ KZT), allowing the budget to cover a dedicated **GIGABYTE GeForce RTX 3060 12 GB**, a **600W PSU**, **32 GB DDR4**, and a **27" QHD IPS monitor** inside a spacious mATX chassis.
- **Total:** **1,032,063 KZT** (`budget.csv`, rows 32–41)
