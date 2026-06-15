# SAP EWM Supply Chain Portfolio

Three interactive supply chain projects built to demonstrate applied SAP Extended Warehouse Management (EWM) knowledge — not just the acronym on a resume.

**[Live Portfolio →](https://nrkaushika.github.io/supply-chain-portfolio)**

---

**Kaushika Naik** · MS Technology Management, UIUC Gies (GPA 3.83) · SAP S/4HANA Supply Chain Execution & EWM  
[LinkedIn](https://linkedin.com/in/kaushika-naik) · [The Flow of Things](https://kaushikanaikr.substack.com) · nrkaushika@gmail.com

---

## Projects

| Project | Description | Link |
|---|---|---|
| Warehouse Slotting Optimizer | Score SKUs by velocity, weight, and cold chain requirements — assign optimal bin zones mirroring SAP EWM putaway strategy | [Open](https://nrkaushika.github.io/supply-chain-portfolio/slotting-optimizer.html) |
| GR to Putaway Simulator V4 | Complete 6-step inbound delivery flow — 6 scenarios, dock scheduling, batch traceability, decision tree | [Open](https://nrkaushika.github.io/supply-chain-portfolio/gr-putaway-simulator.html) |
| Pharma Supply Chain KPI Dashboard | 12 months of simulated pharma KPI data — OTIF, fill rate, MAPE, inventory turnover, and more | [Open](https://nrkaushika.github.io/supply-chain-portfolio/scm-kpi-dashboard.html) |

---

## Project 1 — Warehouse Slotting Optimizer

Most warehouses slot products arbitrarily. Wrong slotting means pickers walk further, pick slower, and cost more. This tool scores every SKU and assigns it to the optimal bin zone.

**Scoring formula:**

```
velocity_score = daily_picks x size_weight x weight_penalty

size_weight:    Small = 1.0  |  Medium = 0.85  |  Large = 0.65
weight_penalty: <= 0.5kg = 1.0  |  <= 1.5kg = 0.85  |  > 1.5kg = 0.65
```

Cold chain items are scored in a completely separate pool and routed to LG05 — they never compete with ambient SKUs for Zone A-D bin assignment. This mirrors how SAP EWM enforces storage type restrictions via the material master.

**Zone assignment maps directly to SAP EWM:**

| Tool zone | SAP EWM storage type | Priority |
|---|---|---|
| Zone A — golden zone | LG01 — highest search sequence priority | 1 — checked first |
| Zone B — mid-velocity | LG02 | 2 |
| Zone C — low-velocity | LG03 | 3 |
| Zone D — bulk/slow | LG04 | 4 |
| Cold chain | LG05 — isolated, temperature-controlled | Separate pool entirely |

**Five tabs:** SKU input, slot assignments, warehouse floor map, analysis charts, EWM logic reference.

**SAP transactions and config:** LX01, LS01N, LX04, SPRO (storage type search sequence, putaway strategy)

---

## Project 2 — GR to Putaway Simulator V4

Walk through a complete Goods Receipt (GR) to putaway flow in SAP EWM. Every step shows the real SAP transaction code and explains the system decision logic behind it.

**Six scenarios:**

| Scenario | Storage type routing | Key SAP logic |
|---|---|---|
| Normal GR | LG01 (Zone A) | Standard putaway, storage type search sequence |
| Damaged goods | LG09 (QI hold) | Quality Management hold, QA32 usage decision required |
| Short delivery | LG01, partial WT | 5% tolerance check, delivery stays open, VL32N |
| Over-delivery | LG10 (hold zone) | GR blocked, PO amendment (ME22N) required before putaway |
| Cold chain | LG05 (2-8C) | Ambient zones bypassed, temperature log recorded |
| Near-expiry / FEFO | LG08 (FEFO zone) | Shelf life < 90 days, First Expiry First Out enforced |

**What V4 adds over V3:**
- Full date and time arrival scheduling (mandatory for Yard Management door assignment)
- Door classification — inbound, outbound, flexible, cross-dock — with live occupancy status
- Dock conflict detection and Yard Waiting Zone (Y-03) pre-assignment
- Over-delivery scenario with full PO amendment flow
- Field validation throughout — vendor from dropdown, system-generated delivery number, condition and quantity cross-checks with shake animation
- Batch number tracked across all 6 steps with visual tracker

**SAP terminology note:** SAP EWM uses Warehouse Task (WT), not Transfer Order (TO). Transfer Order and LT0A belong to legacy SAP WM (LE-WM). EWM uses WT grouped into Warehouse Orders (WO). Transaction: /SCWM/TORD.

**SAP transactions:** VL31N, /SCWM/PRDI, MIGO, /SCWM/GR, /SCWM/TORD, /SCWM/TO_CONF, QA32, ME22N, MIRO, MB56, LX04, VL32N

**Previous versions kept for iteration reference:**
- [V3](https://nrkaushika.github.io/supply-chain-portfolio/ewm-gr-putaway-simulator-v3.html) — scenario selector, FEFO routing, batch traceability, decision tree
- [V2](https://nrkaushika.github.io/supply-chain-portfolio/ewm-gr-putaway-simulator.html) — quality hold routing, auto weight calculation, short delivery

---

## Project 3 — Pharma Supply Chain KPI Dashboard

Twelve months of simulated pharma supply chain data with a realistic narrative — not flat numbers.

**The story in the data:**
- February — supplier delay, OTIF drops to 81.4%
- April — flu season demand spike, Days of Supply jumps to 52, MAPE hits 14.1%
- July — new vendor onboarded, OTIF hits 95.8% for the first time
- October — demand planning miss, MAPE spikes to 18.3%, inventory turnover drops to 6.8x

**Six KPIs with SAP system context:**

| KPI | Full form | Target | Benchmark source | SAP module |
|---|---|---|---|---|
| OTIF | On Time In Full | >= 95% | Gartner / GPO contracts | EWM + SD |
| Fill Rate | Order fill rate | >= 95% | HDA pharma standards | SD |
| Inv. Turnover | Inventory turnover | >= 8x | Deloitte Life Sciences | MM-IM + FI-CO |
| DoS | Days of Supply | <= 45 days | FDA safety stock guidance | MM MRP (MD04) |
| MAPE | Mean Absolute % Error | < 10% | IBF world class | SAP IBP / APO |
| Cycle Time | Order cycle time | <= 2.5 days | HDA distribution standards | SD + EWM |

**Data note:** All values are simulated based on real pharma industry benchmark ranges. Targets are grounded in Gartner, APICS, HDA, and IBF standards. In a real implementation this dashboard would connect to live SAP S/4HANA data via OData APIs and SAP Analytics Cloud.

---

## SAP EWM Concepts Demonstrated

- **Storage Type Search Sequence** — LG01 checked first at putaway, first match wins, configured in SPRO
- **Warehouse Task (WT)** — EWM movement instruction (not Transfer Order — that is legacy WM), confirmed via RF scanner, grouped into Warehouse Orders (WO), transaction /SCWM/TORD
- **Quality Management (QM) Hold** — damaged stock routed to LG09, segregated from unrestricted, QA32 usage decision required
- **FEFO (First Expiry First Out)** — near-expiry stock routed to LG08, picked before standard stock regardless of receipt date
- **Batch Traceability** — batch tracked from GR through every WT to bin confirmation, pharma compliance, MB56 where-used list
- **Yard Management (YM)** — door classification, dock scheduling, conflict detection, yard waiting zone pre-assignment
- **Over-delivery handling** — GR blocked at tolerance breach, stock quarantined in LG10, PO amendment via ME22N required

---

## Currently Building

Integrating all three projects into a connected end-to-end supply chain simulation — slotting optimizer zone assignments feed directly into GR putaway routing, outcomes tracked in the KPI dashboard. Also building a fourth project: an Inventory Optimization Model covering Economic Order Quantity (EOQ), safety stock, and reorder point calculations.

This mirrors how SAP EWM putaway strategy (SPRO), inbound execution (EWM), and business performance measurement (IBP, MM-IM, SD) connect in a real S/4HANA implementation.

---

## Tech Stack

Pure HTML, CSS, and vanilla JavaScript. No frameworks, no libraries, no backend. Single file per project, opens in any browser, hosted on GitHub Pages.

---

## Files

| File | Description |
|---|---|
| `index.html` | Portfolio home page |
| `slotting-optimizer.html` | Warehouse Slotting Optimizer |
| `gr-putaway-simulator.html` | GR to Putaway Simulator (stable URL, current version V4) |
| `ewm-gr-putaway-simulator-v4.html` | V4 source file |
| `ewm-gr-putaway-simulator-v3.html` | V3 — kept for iteration reference |
| `ewm-gr-putaway-simulator.html` | V2 — kept for iteration reference |
| `scm-kpi-dashboard.html` | Pharma Supply Chain KPI Dashboard |
| `kaushika.jpg` | Profile photo |
| `README.md` | This file |
