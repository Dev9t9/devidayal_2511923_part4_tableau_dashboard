# Part 4 — Tableau Executive Dashboard & Data Storytelling

An executive sales dashboard for a retail leadership team, built in Tableau, that turns 4,200
orders into a clear story about where profit is made, where it leaks, and what to do.

## 1. Business problem summary
Leadership needs one executive dashboard to monitor sales, profitability, customer segments,
category performance, shipping, discount impact, and return patterns — and to identify business
opportunities and risks. The dashboard must tell a connected story and support decisions, not be
a set of unrelated charts.

## 2. Dataset description
`data/dashboard_sales_data.xlsx` — 4,200 orders, Jan 2024 – Dec 2025, 20 columns:
- **Date fields:** `order_date`, `ship_date`.
- **Geographic:** `region` (East/North/South/West), `state`, `city`.
- **Categorical:** `customer_segment` (Consumer/Corporate/Home Office), `category`
  (Furniture/Office Supplies/Technology), `sub_category`, `ship_mode`, `campaign_channel`.
- **Numerical measures:** `sales`, `quantity`, `discount` (0–0.35), `profit`, `delivery_days`
  (0–9), `customer_rating` (1–5).
- **Binary/flag:** `return_flag` (1 = returned).
- **Totals:** sales ₹217.0M, profit ₹33.3M, margin 15.3%, return rate 4.5%.

## 3. Tableau workbook description
`tableau/executive_dashboard.twbx` is a packaged workbook (data embedded) containing seven
worksheets — Sales Trend, Regional Performance, Category Profitability, Customer Segment,
Shipping Performance, Discount vs Profit, Return Analysis — assembled into one interactive
executive dashboard with KPI cards, filters, and a highlight action.

## Tools used
- Tableau (Desktop / Public) — calculated fields, worksheets, interactive dashboard with filters and a highlight action.
- Microsoft Excel as the data source; markdown for documentation.

## 4. Calculated fields created (Task 2)
Created in Tableau (exact formulas):

| Field | Tableau formula | Meaning |
|---|---|---|
| **Profit Margin** | `SUM([Profit]) / SUM([Sales])` | profit as a share of sales |
| **Cost** | `[Sales] - [Profit]` | implied cost per order |
| **Average Order Value** | `SUM([Sales]) / COUNTD([Order Id])` | sales per order |
| **Return Rate** | `SUM([Return Flag]) / COUNT([Order Id])` | returned orders ÷ total orders |
| **Shipping Delay Bucket** | see below | delivery days grouped into bands |

```
// Shipping Delay Bucket
IF   [Delivery Days] = 0 THEN "Same day"
ELSEIF [Delivery Days] <= 2 THEN "1-2 days (Fast)"
ELSEIF [Delivery Days] <= 4 THEN "3-4 days (Standard)"
ELSE "5+ days (Slow)"
END
```
A sixth helper, **Loss-Making Order** = `IF [Profit] < 0 THEN "Loss" ELSE "Profit" END`, flags the
288 unprofitable orders for the risk view.

## 5. Dashboard components
- Clear title ("Executive Sales Dashboard").
- 7 views (Task 3) + at least 3 KPI cards (Total Sales, Profit Margin, Return Rate).
- 3+ interactive filters (Region, Category, Customer Segment, Date).
- 1+ action: clicking a region/category highlights it across the dashboard.
- Clean layout, consistent diverging colour for margin (red = low, green = high), minimal clutter.

## 6. Filters and interactions used
Quick filters for **Region, Category, Customer Segment, Order Date (month/year)**, and a **Ship
Mode** filter on the shipping view. A dashboard **highlight action** on category/region links the
views so selecting one updates the story everywhere.

## 7. Key business insights
Full detail in `outputs/business_insights.md`. Headlines:
- **Technology drives ~84% of profit** (18.2% margin); **Furniture is barely profitable** (6.9%).
- **Discounts above ~20% destroy profit** — beyond 30%, 72% of orders lose money; 288 orders (6.9%) are already loss-making.
- **Furniture (7.7%) and Home Office (5.7%) return the most.**
- Sales are **stable, +4.3% YoY**; regions are evenly profitable, South leads on volume.

## 8. Dashboard story summary
The story (`outputs/dashboard_story.md`) connects the views: a stable revenue backdrop, a
Technology-vs-Furniture profit split, a discount-driven profit leak, and concentrated returns —
leading to one action list headed by **discount discipline** and **Furniture re-pricing**.

## 9. Assumptions and limitations
- Shows **association, not causation** (e.g. discount ↔ profit).
- **Two years** of data — limited seasonality separation.
- Minor missing values in `customer_rating` (32) and `campaign_channel` (24) are excluded from
  the views that use them.
- Order-level aggregates flag *where* to look, not the root cause of each return/loss.

## 10. Screenshots included
- `screenshots/full_dashboard.png` — complete executive dashboard.
- `screenshots/sales_trend_view.png` — sales trend line chart.
- `screenshots/regional_performance_view.png` — region/state performance.
- `screenshots/category_profitability_view.png` — category/sub-category profitability.
- `screenshots/filter_interaction_view.png` — evidence of a filter/highlight interaction.

## Repository structure
```
part4_tableau_dashboard/
├── data/dashboard_sales_data.xlsx
├── tableau/executive_dashboard.twbx
├── outputs/
│   ├── dashboard_story.md
│   ├── business_insights.md
│   └── chart_selection_justification.md
├── screenshots/
│   ├── full_dashboard.png
│   ├── sales_trend_view.png
│   ├── regional_performance_view.png
│   ├── category_profitability_view.png
│   └── filter_interaction_view.png
└── README.md
```
