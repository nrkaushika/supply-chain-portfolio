paste this?

# Warehouse Slotting Optimizer
An interactive supply chain tool that scores SKUs by pick velocity, weight, and cold chain requirements — then assigns them to optimal warehouse zones. Built to demonstrate applied SAP EWM knowledge.
**[Live Demo →](https://nrkaushika.github.io/warehouse-slotting-optimizer)**
---
## What it does
Most warehouses slot products arbitrarily. Wrong slotting means pickers walk further, pick slower, and cost more. This tool solves that by scoring every SKU and recommending a zone assignment based on three factors:
- **Pick velocity** — how many times per day is this SKU picked?
- **Weight** — heavier items should be at waist height, not top shelf
- **Size + cold chain** — large items need floor slots; cold chain items need temperature-controlled sections
The zone system maps directly to how SAP EWM structures putaway:
| Optimizer zone | EWM equivalent | Priority |
|---|---|---|
| Zone A (golden zone) | Storage type with highest search sequence | 1 — checked first |
| Zone B | Secondary storage type | 2 |
| Zone C | Tertiary storage type | 3 |
| Zone D | Bulk / overflow storage | 4 |
---
## Features
- **SKU input** — add any product with daily pick count, weight, size, and category
- **Slot assignments** — velocity scores + zone + bin ID + rationale for each SKU
- **Warehouse map** — visual 32-bin floor plan showing zone distribution
- **Analysis** — pick frequency bar charts + zone distribution
- **EWM logic tab** — maps every optimizer concept to the corresponding SAP transaction
---
## Scoring formula

velocity_score = daily_picks × size_weight × weight_penalty × cold_chain_bonus
size_weight: Small = 1.0 | Medium = 0.85 | Large = 0.65 weight_penalty: ≤0.5kg = 1.0 | ≤1.5kg = 0.85 | >1.5kg = 0.65 cold_chain_bonus: cold chain = 1.1 | standard = 1.0

Zone cutoffs are relative to the highest-scoring SKU in the dataset, so the optimizer adapts to whatever product mix you feed it.
---
## How to use
1. Open `index.html` in any browser — no dependencies, no build step required
2. Modify the default SKU list or add your own
3. Click **Run slotting optimizer**
4. Review zone assignments, warehouse map, and EWM logic tab
---
## Why I built this
I hold the **SAP S/4HANA Supply Chain Execution & EWM** certification (SAP Learning) and wanted to demonstrate that knowledge practically — not just as a line on a resume.
Slotting is one of the highest-impact, lowest-cost levers in warehouse operations. A well-slotted warehouse reduces picker travel by 20–40%, directly cutting labor cost per order. This tool makes that analysis accessible without needing an EWM system to test against.
---
## Tools in this portfolio
| Tool | Description | Link |
|---|---|---|
| Warehouse Slotting Optimizer | Score SKUs → assign optimal bin zones | [Open](https://nrkaushika.github.io/warehouse-slotting-optimizer/index.html) |
| EWM GR → Putaway Simulator | Full inbound delivery flow with EWM transaction logic | [Open](https://nrkaushika.github.io/warehouse-slotting-optimizer/ewm-gr-putaway-simulator.html) |
---
## Tech stack
Pure HTML + CSS + vanilla JavaScript. No frameworks, no libraries, no backend. Single file — open it anywhere.
---
## About
Built by **Kaushika Naik Ramavatu** — MS Technology Management, Gies College of Business, UIUC.
Supply chain operations + SAP EWM + strategy. Writing at [The Flow of Things](https://kaushikanaikr.substack.com).
[LinkedIn](https://linkedin.com/in/kaushika-naik-ramavatu) · [Substack](https://kaushikanaikr.substack.com)
