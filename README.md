# 📊 Sales Pipeline Dashboard

A Power BI dashboard for tracking sales pipeline performance, stage gap analysis, and agent productivity.

---

## 📌 Overview

The **Sales Pipeline Dashboard** is an interactive Power BI reporting tool designed to monitor, analyze, and evaluate the performance of sales agents across different stages of the sales pipeline.

It transforms raw sales data into clear visual insights that help managers understand:
- How leads move across pipeline stages
- Where leads drop off or get stuck
- How individual agents are performing
- Where the biggest conversion gaps exist

---

## 📄 Pages

| Page | Description |
|------|-------------|
| **Leads Over Stages** | Main overview page — donut chart showing client distribution across all pipeline stages, KPI cards, and the leads dropped gauge |
| **Interest Gap (A)** | Clients stuck between Interested/Follow-Up and Potential to Close |
| **Commitment Gap (B)** | Clients stuck between Potential to Close and Meeting Scheduled |
| **Show-Up Gap (C)** | Clients stuck between Meeting Scheduled and Meeting Done |
| **Closing Gap (D)** | Clients stuck between Meeting Done and Done Deal |
| **Lost Deals** | Analysis of leads that resulted in a lost deal |
| **Leads Dropped** | Clients who moved from a positive stage to a negative stage |
| **Clients History** | Full historical log of client status changes |
| **Direct to Meeting** | Clients who skipped early stages and went directly to a meeting |

---

## 🔍 Key Features

### Pipeline Stage Distribution
A donut chart on the main page shows how many clients have passed through each stage at any point in the sales process — not just their current stage. The center number represents the total pipeline size.

### Gap Analysis (A → D)
Four critical transition gaps are measured across the pipeline:

| Gap | Transition | What it reveals |
|-----|-----------|-----------------|
| **Interest Gap (A)** | Interested/Follow-Up → Potential to Close | Weak qualification or follow-up |
| **Commitment Gap (B)** | Potential to Close → Meeting Scheduled | Difficulty securing appointments |
| **Show-Up Gap (C)** | Meeting Scheduled → Meeting Done | High no-show rate or poor reminders |
| **Closing Gap (D)** | Meeting Done → Done Deal | Weak closing or pricing objections |

Each gap page shows:
- The percentage gap (color-coded: 🟢 green if below 50%, 🔴 red if above 50%)
- Number of clients stuck in that gap
- A filterable client list with drill-through to individual client history

### Leads Dropped Gauge
Monitors how many leads moved from a positive stage (Interested, Potential to Close, Meeting Scheduled, Meeting Done) to a negative stage (Not Interested, Low Budget). A target line is set at **40%** of total leads — if exceeded, the gauge turns red.

### Drill-Through
Right-click any GAP card, Lost Deals metric, or Leads Dropped indicator on the main page and select **Drill Through** to navigate to the detailed breakdown page for that metric.

### Filters Available
- Filter by Sales Agent
- Filter by Date
- Filter by Stage
- Filter by Campaign
- Filter by Client Name (on detail pages)

---

## 🗃️ Data Model

| Table | Description |
|-------|-------------|
| `Status Changes` | Core fact table — logs every status transition per client (agent, client name, from/to status, date, ID) |
| `Stages` | Lookup table for pipeline stage names |
| `Agents` | Agent name dimension |
| `Workspace Data` | Campaign tags and workspace metadata |
| `Clients GAP A/B/C/D` | Segmented client lists per gap |
| `Clients Dropped` | Clients who dropped to a negative stage |
| `Clients Direct to Meeting` | Clients who went directly to a meeting |
| `Clients Lost Deal` | Clients who ended in a lost deal |
| `@Measurements` | All calculated DAX measures (28+) |

---

## 🧮 Key DAX Measures

```dax
-- Counts clients who have reached a stage at any point (not just current)
Clients Interested Follow Up =
VAR CountFromIFU = CALCULATE(
    DISTINCTCOUNT('Status Changes'[Client Name]),
    REMOVEFILTERS('Stages'),
    'Status Changes'[From Status] = "Interested/Follow Up"
)
VAR CountToIFU = CALCULATE(
    DISTINCTCOUNT('Status Changes'[Client Name]),
    REMOVEFILTERS('Stages'),
    'Status Changes'[To Status] = "Interested/Follow Up"
)
RETURN MAX(CountFromIFU, CountToIFU)
```

```dax
-- Gap percentage between two stages
%GAP(A) = IFERROR(
    DIVIDE(
        [Clients Interested Follow Up] - [Clients Potential To Close],
        [Clients Interested Follow Up]
    ), 0)
```

> **Note:** All stage measures use `REMOVEFILTERS('Stages')` to prevent the Stages table relationship from incorrectly filtering the Status Changes fact table when stages are used on visual axes.

---

## 🚀 Getting Started
1. Clone or download this repository
2. Unzip `sales-pipeline-dashboard.zip`
3. Open `sales-pipeline-dashboard.pbix` in **Power BI Desktop**

---

### Requirements
- Power BI Desktop (latest version recommended)

---

## 📁 Repository Structure
```
sales-pipeline-dashboard/
├── sales-pipeline-dashboard.pbix
├── README.md
└── documentation/
    └── PowerBi_documentation_summary.pptx
```

## 📸 Preview
<img width="661" height="371" alt="image" src="https://github.com/user-attachments/assets/1887aafb-30fb-426a-8964-86b0c56fa333" />

---

## 🔗 Live Demo
[View the live dashboard on Power BI Service](https://app.powerbi.com/view?r=eyJrIjoiNWQ4MzdmN2MtY2U5Ny00NDE1LTljODAtMGM0ZmFmOGQ5ZWY1IiwidCI6ImU3ZTkxMDI3LTk2NTEtNDMxYS04ZWY3LWQyZjI5MWNhODIyMCJ9)

## 👤 Author

**Adam Al-Shenawy**
GitHub: [@adamalshenawy](https://github.com/adamalshenawy)
