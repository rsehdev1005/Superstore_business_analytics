
# Superstore Business Analytics Dashboard

An end-to-end business analytics project analyzing retail sales, customer 
behavior, and product profitability using Python and Power BI.

## Objective

Analyze sales performance, customer retention, and product-level 
profitability for a retail business, and surface actionable insights 
for leadership — going beyond raw dashboards to identify specific 
problem areas.

## Data Source

- **Dataset:** Superstore Sales Dataset (`SuperStore_Sales_DataSet.xlsx`)
- **Size:** 5,901 order line items, spanning January 2019 – December 2020
- **Scope:** 773 unique customers, 1,755 unique products across 
  Furniture, Office Supplies, and Technology categories

## Tools Used

- **Python (pandas & numpy)** — data cleaning and feature engineering
- **Power BI** — data modeling (star schema), DAX measures, and 
  interactive dashboarding
- **Power Query** — table splitting and transformation within Power BI

## Approach

### 1. Data Cleaning (Python & Numpy)
Raw data was cleaned using `scripts/Clean_data.ipynb`:
- Removed fully empty columns and fixed a corrupted column header
- Converted a sparse `Returns` flag into a clean `Is Returned` boolean
- Verified no true duplicate rows or invalid ship dates
- Engineered new features: `Order Processing Time`, `Profit Margin %`, 
  `Customer First Purchase Date`, `Is Repeat Customer` 
- Added a simulated `Discount` column (randomly assigned 0%, 10%, 20%, 
  or 30% per row), since the source data did not include discount 
  information — **this field is synthetic, not observed data**, and is 
  included to illustrate a discount-sensitivity view rather than to 
  represent a real business pattern

### 2. Data Modeling (Power BI)
The cleaned flat table was split into a proper star schema directly in 
Power Query:
- `fact_orders` — transactional grain (Sales, Profit, Quantity, Discount, 
  Order/Ship dates, foreign keys)
- `dim_customer` — one row per customer (Segment, Region, Location, 
  retention flags)
- `dim_product` — one row per product (Category, Sub-Category)
- `dim_date` — built with DAX (`CALENDAR`), marked as the official date 
  table, with Year/Quarter/Month/Month Number columns for time intelligence

### 3. DAX Measures
Key measures built for the analysis: `Total Revenue`, `Total Profit`, 
`Profit Margin %` and `YoY Sales Growth %` (using `SAMEPERIODLASTYEAR`) for Top-N product analysis.

### 4. Dashboard
Four report pages:
1. **Key Insights** — summary of findings
2. **Sales Summary** — cards, sales trend by month/year, 
   sales-by-state map, profit margin vs. revenue trend, total sales to previous year sales, total sales by discount
3. **Customer Analysis** — sales by region/city, top 10 
   customers, repeat customer rate
4. **Product Deep-Dive** — profitability by product, 
   top-selling vs. top-profitable product comparison, orders by ship mode

## Key Insights

- Total sales reached $1.57M across 5,901 transactions and 22K units, with $175.26K in profit — an overall profit margin sites under 12%, worth calling out as thin.

- West region leads at ~$505K, roughly double the lowest-performing South region (~$258K) — signals either a regional growth opportunity or  overconcentration risk.

- Several Furniture/Bookcase show negative profit margins (down to -243%) despite steady unit sales — these are actively losing money, not just underperforming.

- 89% of customers are repeat buyers — strong retention despite probability gaps, the problem is  profitability per sale, not customer loyalty.

- Binders and Paper dominate unit volume but Copiers and Accessories generate the most profit.(volume ≠ profit).

## Dashboard Preview

![Key Insights](screenshots/key_insights.png)
![Sales Summary](screenshots/sales_dashboard.png)
![Customer & Segment Analysis](screenshots/customer_analysis.png)
![Product & Profitability Deep-Dive](screenshots/product_analysis.png)

## Repository Structure
superstore-business-analytics/
├── data/
│ ├── SuperStore_Sales_DataSet.xlsx
│ ├── SuperStore_Cleaned.xlsx
├── scripts/
│ └── clean_data.ipynb
├── dashboard/
│ └── superstore_dashboard.pbix
├── screenshots/
│ ├── key_insights.png
│ ├── sales_dashboard.png
│ ├── customer_analysis.png
│ └── product_analysis.png
└── README.md

## Note on Data Limitations

The `Discount` field was not present in the source data and was 
simulated for this project (random values: 0%, 10%, 20%, 30%). Any 
apparent relationship between discount and profit margin in the 
dashboard is coincidental, not a real business finding, and should 
not be interpreted as an actual driver of profitability.

