# Azure Pricing Models — Cost Estimation for a Three-Tier Web App

Coursework submission for the **Azure Pricing Models Learning Program**. The project builds a monthly cost estimate for a three-tier web application in West Europe, then runs what-if scenarios across billing models (pay-as-you-go vs Reserved Instances), licensing (Azure Hybrid Benefit), storage redundancy (LRS vs GRS), and IaaS vs PaaS.

## Headline numbers

| Option / Scenario | USD/month |
|---|---|
| Pay-as-you-go baseline | **$679.58** |
| Optimized (3-yr RI + Hybrid Benefit + LRS) | **$421.96** |
| Saving | **$257.61 (37.9%)** |

The single biggest lever turned out to be Azure Hybrid Benefit stacked on a 3-year reservation, which cuts the web-tier VM cost by 78.6% without touching the architecture.

---

## Repository contents

```
azure-pricing-models/
├── README.md
├── docs/
│   ├── ARCHITECTURE.md                # 3-Tier Web App, SKU choices, Spot VMs guide
│   ├── DATA_ANALYTICS_ARCHITECTURE.md # Data Analytics stack (Synapse/ADF/ADLS Gen2)
│   ├── OPTIMIZATION_ANALYSIS.md       # PAYG vs RI, Hybrid Benefit, 3-region cost table
│   └── screenshots/                   # Pricing Calculator verification evidence
│       ├── calculator_compute_settings.png
│       └── calculator_data_settings.png
└── estimate/
    └── azure-cost-estimate-west-europe.xlsx   # Cost estimate report (3 sheets)
```

The workbook has three sheets. `Assumptions` holds every unit price with its source and capture date (blue cells are the inputs). `Cost Estimate` is the calculator-style line-item baseline. `Optimization Scenarios` computes the five what-if comparisons with live formulas, so changing any price on the Assumptions sheet recalculates everything.

---

## Reproducing the estimate in the Azure Pricing Calculator

Prices in the workbook are USD list prices captured 11 June 2026; they drift. To regenerate an official export:

1. Open https://azure.microsoft.com/pricing/calculator/ and set region to **West Europe**, currency USD.
2. Add: Virtual Machines (2× D2s_v5, Windows, 730 hrs), Managed Disks (2× P10), Load Balancer (Standard, 5 rules, 1,536 GB processed), Azure Cache for Redis (Standard C1), Azure SQL Database (Single DB, DTU, Standard S2), Storage Account (GPv2, Hot, GRS, 1,024 GB), Bandwidth (1,024 GB internet egress).
3. Toggle "Savings options" on the VM block to read the 1-yr/3-yr reservation rates, and tick Azure Hybrid Benefit to see the license effect. *(See `docs/screenshots/calculator_compute_settings.png`)*.
4. Verify database and storage account settings *(See `docs/screenshots/calculator_data_settings.png`)*.
5. Export to Excel from the calculator and drop the file into `estimate/` alongside this workbook.

---

## Assignment task mapping

| Task | Where |
|---|---|
| 1. Define architecture | `docs/ARCHITECTURE.md` (diagram + component list) |
| 2. Configure compute & Spot VMs | Workbook `Assumptions` + `Cost Estimate`; Spot VMs & B- vs D-series in `docs/ARCHITECTURE.md` |
| 3. Data and storage (DTU vs vCore, LRS/GRS) | ARCHITECTURE.md "Why these SKUs"; Scenario 2 in OPTIMIZATION_ANALYSIS.md |
| 4. Networking estimate (1 TB egress) | `Cost Estimate` bandwidth line |
| 5. PAYG vs RI, IaaS vs PaaS | Scenarios 1 and 4 in OPTIMIZATION_ANALYSIS.md |
| 6. Azure Hybrid Benefit | Scenario 3 |
| 7. Report and export | `estimate/azure-cost-estimate-west-europe.xlsx` |
| 8. Breadth: Secondary Stack | `docs/DATA_ANALYTICS_ARCHITECTURE.md` (Data Analytics pipeline) |
| 9. Multi-region Pricing | "Regional Cost Comparison Analysis" in `docs/OPTIMIZATION_ANALYSIS.md` (3-region side-by-side table) |
