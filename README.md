# Urban Trail — Competitive Pricing Intelligence Dashboard

A Power BI dashboard built to track daily price movements across e-commerce channels, benchmark against competitors, and flag margin erosion for a multi-category retail brand (apparel, footwear, bags, and accessories).

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![Excel](https://img.shields.io/badge/Excel-217346?style=flat&logo=microsoft-excel&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-005A9C?style=flat)

---

## Overview

Urban Trail sells across five channels — Website, Amazon, Flipkart, Myntra, and AJIO — with prices that shift daily due to promotions, stock-outs, and channel-specific discounting. This dashboard consolidates that fragmented pricing data into a single source of truth so the pricing/category team can:

- Spot which channel is undercutting MRP/BAU on any given day
- Quantify how far each channel's price has drifted from the internal BAU (Business-As-Usual) price
- Track dealer margin erosion over time as competitor and channel prices move
- Benchmark Average Selling Price (ASP) against 10 named competitor brands (Nike, Adidas, Puma, Woodland, Decathlon, etc.)

## Dashboard Pages

### 1. Daily Price Comparison
SKU-level matrix showing MRP, CDP, BAU, and live prices across all 5 channels, with a **BAU Diff** column per channel (price minus BAU) to instantly surface which channel is discounting hardest. Includes a computed **Min SKU Rate**, **Min SKU Diff**, and **Drop %** to rank the worst-eroding SKUs, conditionally formatted (red heatmap) by discount severity.

**Slicers:** Month/Day, Week of Month, Channel, Category, Sub Category

### 2. Date-Wise Price Trend
Time-series view of Min Price, CDP, BAU, and **Dealer Margin** trended daily across the selected month, paired with a full transaction-level table (Date, SKU, Category, all channel prices, Daily Min Price, Price Drop, Price Drop %, Dealer Margin). Built to catch sudden margin dips day-over-day rather than waiting for a monthly rollup.

**Slicers:** Month, Week of Month, Category, Sub Category, Date range

### 3. Competitor Benchmarking
Weekly ASP (Average Selling Price) per base unit, tracked by brand and date, visualized as a multi-line trend across 10 competitor brands to show where Urban Trail sits in the competitive price band.

**Slicers:** Month, Week of Month, Brand, Category, Date range

## Data Model

The dashboard is powered by a 5-table Excel data model:

| Table | Purpose | Grain |
|---|---|---|
| `Daily Price Comparison` | Channel prices by SKU, date, and time-of-day snapshot | SKU × Date × Time |
| `Competitor Price Input` | Competitor brand pricing scraped from Amazon/Flipkart | Brand × SKU × Date |
| `Pricing Master` | Internal BAU / CDP / MRP by SKU and validity period | SKU × Date Range |
| `SKU Category Master` | SKU → Category → Sub-Category mapping | SKU |
| `Channel Links` | Product URLs per SKU per channel | SKU |

All tables join on **SKU**, with `Pricing Master` date ranges resolved against the transaction date using DAX (`CALCULATE` + date-in-range logic) to pull the correct BAU/CDP/MRP for any given day.

## Key DAX Measures

- `Website/Amazon/Flipkart/Myntra/AJIO BAU Diff` = Channel Price − Active BAU Price
- `Daily Min Price` = MIN across all live channel prices for a SKU/date
- `Price Drop` = MRP − Daily Min Price
- `Price Drop %` = Price Drop / MRP
- `Dealer Margin` = BAU Price − CDP Price
- `ASP Per Base Unit` = Average competitor selling price normalized to a common unit basis

## Tech Stack

- **Power BI Desktop** — data model, DAX measures, report pages
- **Excel** — source data staging and SKU master management
- **Power Query** — data cleaning, date-range joins, channel price parsing (handling "Out of Stock" / "Check Price" text states in numeric columns)

## Repo Contents

```
├── Urban_Trail_Master_File.xlsx     # Source data (5 linked tables)
├── Urban_Trail_Dashboard.pbix       # Power BI report
└── README.md
```

## Note on Data

Product, brand, and pricing data in this repo are illustrative/synthetic, structured to mirror a real production pricing feed. The dashboard design, data model, and DAX logic are original work.

---

*Built during a data/pricing analytics internship to automate what was previously a manual, spreadsheet-based daily price-check process.*
