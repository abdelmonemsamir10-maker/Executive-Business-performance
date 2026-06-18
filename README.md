# 📊 Business Performance Analysis — Apparel Manufacturing



## 📁 Project Structure

```
📦 Business-Performance-Analysis
 ┣ 📊 Business_Analyst_Task_Data_Set.xlsx     ← Raw Data (3 Sheets)
 ┣ 📈 Dice_Dashboard.pbix                     ← Power BI Dashboard
 ┣ 📑 Business_Performance_Analysis.pptx      ← Executive Presentation
 ┗ 📄 README.md
```

---

## 🎯 Project Objective

Analyze purchase orders, production performance, and delivery data to:
- Identify top-performing customers and products
- Measure production efficiency and on-time delivery
- Highlight risks and provide actionable recommendations
- Build an interactive Power BI dashboard for business stakeholders

---

## 📂 Data Sources

| Sheet | Rows | Period | Description |
|---|---|---|---|
| Purchase_Orders | 3,024 | 2023–2025 | Orders by customer, category, style |
| Production_Data | 180 | Jan–Jun 2025 | Planned vs actual production per unit |
| Purchase_Order_Detail | 9,091 | 2023–2025 | Granular order-level detail |

---

## 🔧 Tools & Technologies

| Tool | Usage |
|---|---|
| **Power BI Desktop** | Dashboard, Data Model, Visualizations |
| **Power Query** | Data cleaning & transformation (ETL) |
| **DAX** | Custom KPI measures |
| **Microsoft Excel** | Source data |
| **PowerPoint** | Executive presentation |

---

## 🧹 Data Cleaning (Power Query)

- Converted `Month` text column to proper `Date` format
- Added `Product Category` column to Production_Data via 2-letter Style prefix mapping (`TS` → T-Shirts, `HD` → Hoodies, etc.)
- Added `Category_Prefix` column to Purchase_Orders for model joining
- Standardized column names across all 3 sheets
- Documented 4 data quality issues with severity ratings

**Key Assumption:**
> `Turnover_USD` used as single source of truth (370 rows show minor variance vs Pieces × FOB, max $7.19 — likely rounding)

---

## 🗂️ Data Model (Star Schema)

```
         Category_Dim (1)
        /       |        \
       ▼        ▼         ▼
  Production  Purchase   PO_Detail
   _Data (*)  _Orders(*)   (*)
```

- **Category_Dim** — Bridge table (7 product categories) to resolve Many-to-Many relationships
- All relationships: One-to-Many, Single cross-filter direction

---

## 📐 DAX Measures

| Measure | Formula |
|---|---|
| Total Turnover | `SUM(Purchase_Orders[Turnover_USD])` |
| Total Pieces | `SUM(Purchase_Orders[Pieces Ordered])` |
| Total Orders | `COUNTROWS(Purchase_Orders)` |
| Avg FOB | `AVERAGE(Purchase_Orders[FOB_USD])` |
| Active Customers | `DISTINCTCOUNT(Purchase_Orders[Customer])` |
| Production Achievement % | `DIVIDE(SUM(Actual), SUM(Planned)) * 100` |
| Production Gap | `SUM(Planned) - SUM(Actual)` |
| OTD % | `DIVIDE(COUNTROWS(on-time filter), COUNTROWS(all), 0) * 100` |
| Avg Days Late | `AVERAGEX(Production_Data, DATEDIFF(Customer_FOB, Actual_FOB, DAY))` |
| Turnover PY | `CALCULATE([Total Turnover], SAMEPERIODLASTYEAR(...))` |
| YoY Growth % | `DIVIDE([Total Turnover] - [Turnover PY], [Turnover PY]) * 100` |

---

## 📊 Dashboard Pages

| Page | Description |
|---|---|
| 1️⃣ Executive Summary | KPI cards, Turnover by Year, Category breakdown, Customer contribution |
| 2️⃣ Customer Analysis | Volume & revenue ranking, YoY growth, FOB comparison, detail table |
| 3️⃣ Style Performance | Top styles by revenue & volume, FOB analysis, Turnover by year |
| 4️⃣ Production Performance | Achievement % by unit, Planned vs Actual, OTD by unit, Gap by customer |
| 5️⃣ OTD & Delivery | OTD trend, Days late by unit, On-time vs late, Detail table |
| 6️⃣ Risk & Early Warning | Risk signals, OTD trend, Production gap, Early warning table |

---

## 📈 Key KPIs

| KPI | Value |
|---|---|
| 💰 Total Turnover (3Y) | $393.70M |
| 📦 Total Pieces Ordered | 75M |
| 📋 Total Orders | 3,024 |
| 🏷️ Avg FOB per Piece | $5.25 |
| 🤝 Active Customers | 12 |
| 🏭 Production Achievement | 91.6% |
| 📉 Production Gap | 487K pieces |
| 🚨 OTD Rate | 27.8% |
| ⏱️ Avg Days Late | 5.40 days |

---

## 💡 Key Insights

1. **🔴 OTD Crisis** — Only 27.8% of shipments delivered on time vs 95% industry benchmark. 72.2% of shipments are late.
2. **🟠 Production Shortfall** — 487K pieces gap between planned and actual production (8.37% shortfall).
3. **🟠 Customer Concentration Risk** — Top 3 customers (Decathlon, Zara, Carrefour) = 28% of total revenue.
4. **🟡 Unit F Underperformance** — Lowest achievement at 78.5% and highest avg delay of 15 days.
5. **🟢 Healthy Revenue Growth** — $128.5M (2023) → $134.7M (2024) = +4.8% YoY growth.
6. **🟢 Balanced Category Mix** — All 7 categories contribute ~14% each — good diversification.

---

## 🎯 Recommendations

| # | Action | Priority |
|---|---|---|
| 01 | Fix OTD — audit Unit F, implement daily tracking | 🔴 URGENT |
| 02 | Close production gap — review capacity planning | 🟠 HIGH |
| 03 | Diversify customer base — target 3–5 new customers | 🟠 STRATEGIC |
| 04 | Promote high-FOB styles (Jackets, Hoodies) | 🟢 GROWTH |
| 05 | Align Style codes across Production & Sales systems | 🔵 PROCESS |
| 06 | Monthly Power BI review cadence with leadership | ✅ DONE |

---

