# SAP EWM Supply Chain Portfolio

An interactive supply chain portfolio built to demonstrate applied SAP Extended Warehouse Management (EWM) knowledge — not just the acronym on a resume.

**[Portfolio Home →](https://nrkaushika.github.io/warehouse-slotting-optimizer)**

---

## Tools

| Tool | Description | Link |
|---|---|---|
| Warehouse Slotting Optimizer | Score SKUs by velocity, weight, and cold chain — assign optimal bin zones | [Open](https://nrkaushika.github.io/warehouse-slotting-optimizer/slotting-optimizer.html) |
| GR to Putaway Simulator V4 | Full inbound delivery flow — 6 scenarios, dock scheduling, decision tree | [Open](https://nrkaushika.github.io/warehouse-slotting-optimizer/ewm-gr-putaway-simulator-v4.html) |
| Pharma Supply Chain KPI Dashboard | 12 months of simulated pharma KPI data — OTIF, fill rate, MAPE, and more | [Open](https://nrkaushika.github.io/warehouse-slotting-optimizer/scm-kpi-dashboard.html) |

---

## Tool 1 — Warehouse Slotting Optimizer

Most warehouses slot products arbitrarily. Wrong slotting means pickers walk further, pick slower, and cost more. This tool scores every SKU and assigns it to the optimal bin zone.

**Scoring formula:**

```
velocity_score = daily_picks × size_weight × weight_penalty

size_weight:    Small = 1.0 | Medium = 0.85 | Large = 0.65
weight_penalty: ≤0.5kg = 1.0 | ≤1.5kg = 0.85 | >1.5kg = 0.65

Cold chain items scored independently in their own storage pool (LG05) —
never compete with ambient SKUs for Zone A–D assignment.
```

**Zone assignment maps to SAP EWM:**

| Zone | EWM storage type | Priority |
|---|---|---|
| Zone A — golden zone | LG01 — highest search sequence priority | 1 — checked first |
| Zone B — mid-velocity | LG02 | 2 |
| Zone C — low-velocity | LG03 | 3 |
| Zone D — bulk/slow | LG04 | 4 |
| ❄ Cold chain | LG05 — isolated, temperature-controlled | Separate pool |

---

## Tool 2 — GR to Putaway Simulator (V4)

Walk through a complete Goods Receipt (GR) to putaway flow in SAP EWM. Every step shows the real transaction code and explains the system decision logic.

**Six scenarios:**

| Scenario | Routing | Key EWM logic |
|---|---|---|
| ✅ Normal GR | LG01 (Zone A) | Standard putaway — storage type search sequence |
| ⚠️ Damaged goods | LG09 (QI hold) | Quality Management (QM) hold — QA32 usage decision required |
| 📦 Short delivery | LG01, partial TO | Tolerance check — delivery stays open if > 5% short |
| 📫 Over-delivery | LG10 (hold zone) | GR blocked — PO amendment (ME22N) + purchasing approval required |
| ❄️ Cold chain | LG05 (2–8°C) | Ambient zones bypassed — temperature log recorded |
| ⏰ Near-expiry | LG08 (FEFO zone) | Shelf life < 90 days → FEFO (First Expiry First Out) routing |

**V4 additions over V3:**
- Full date + time arrival scheduling (mandatory for Yard Management door assignment)
- Door classification — inbound, outbound, flexible, cross-dock — with live occupancy status
- Dock conflict detection — overlap warning + Yard Waiting Zone pre-assignment
- Over-delivery scenario with PO amendment flow
- Field validation — vendor from dropdown, system-generated delivery number, condition/quantity cross-checks
- Batch number tracked across all 6 steps with visual tracker

**Previous versions kept for iteration reference:**
- [V3](https://nrkaushika.github.io/warehouse-slotting-optimizer/ewm-gr-putaway-simulator-v3.html) — scenario selector, FEFO routing, batch traceability, decision tree
- [V2](https://nrkaushika.github.io/warehouse-slotting-optimizer/ewm-gr-putaway-simulator.html) — quality hold routing, auto weight calculation, short delivery

---

## Tool 3 — Pharma Supply Chain KPI Dashboard

Twelve months of simulated pharma supply chain data with a realistic narrative — not flat textbook numbers.

**Story baked into the data:**
- **February** — supplier delay tanks OTIF to 81.4%
- **April** — flu season demand spike, Days of Supply jumps to 52
- **July** — new vendor onboarded, OTIF hits 95.8% for the first time
- **October** — demand planning miss, 18.3% MAPE, inventory turnover drops to 6.8×

**Six KPIs tracked with SAP context:**

| KPI | Full form | Target | SAP context |
|---|---|---|---|
| OTIF | On Time In Full | ≥ 95% | Delivery reliability reports — EWM/SD |
| Fill Rate | Order fill rate | ≥ 95% | Goods issue confirmation vs order qty |
| Inv. Turnover | Inventory turnover | ≥ 8× | COGS ÷ avg inventory — MM-IM |
| DoS | Days of Supply | ≤ 45 days | MD04 stock/requirements list |
| MAPE | Mean Absolute % Error | < 10% | APO/IBP demand planning |
| Cycle Time | Order cycle time | ≤ 2.5 days | Delivery processing time — SD/EWM |

---

## EWM concepts demonstrated

- **Storage Type Search Sequence** — LG01 checked first at putaway, first match wins, configured in SPRO
- **Transfer Order (TO)** — EWM's internal movement instruction, confirmed via RF (Radio Frequency) scanner
- **Quality Management (QM) Hold** — damaged stock bypasses normal putaway, goes to LG09, QA32 usage decision required
- **FEFO (First Expiry First Out)** — near-expiry stock routed to LG08, picked before standard stock
- **Batch Traceability** — batch tracked from GR through TO, bin determination, and confirmation — pharma compliance
- **Yard Management (YM)** — door classification, dock scheduling, conflict detection, waiting zone assignment
- **Over-delivery handling** — GR blocked at tolerance breach, PO amendment via ME22N required

---

## What's next

Currently working on integrating all three tools into a connected supply chain simulation — slotting optimizer zone assignments feed directly into the GR simulator's putaway routing, outcomes measured in the KPI dashboard. This mirrors how these systems connect in real SAP: putaway strategy configuration is downstream of slotting decisions, and both show up in OTIF and fill rate.

---

## Tech stack

Pure HTML + CSS + vanilla JavaScript. No frameworks, no libraries, no backend. Single file per tool — open in any browser.

---

## About

Built by **Kaushika Naik Ramavatu** — MS Technology Management, Gies College of Business, University of Illinois Urbana-Champaign (UIUC). SAP S/4HANA Supply Chain Execution and EWM coursework. Writing about supply chain operations at [The Flow of Things](https://kaushikanaikr.substack.com).

[LinkedIn](https://linkedin.com/in/kaushika-naik-ramavatu) · [Substack](https://kaushikanaikr.substack.com) · [Portfolio](https://nrkaushika.github.io/warehouse-slotting-optimizer)
