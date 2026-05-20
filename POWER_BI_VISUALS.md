# Power BI Result Visualization (Company M)

This guide shows how to turn the Company M SQL exports into a Power BI report with clear, decision-oriented visuals.

## Data export (SSMS → CSV)

Export each query result to CSV.

### Financials

```sql
USE [Company M];
SELECT *
FROM dbo.[Financials]
ORDER BY year;
```

### Shares sold

```sql
USE [Company M];
SELECT *
FROM dbo.[Shares sold]
ORDER BY year;
```

Save as:
- `Financials.csv`
- `SharesSold.csv`

> Note: CSV exports are not committed to GitHub (see `.gitignore`). Keep them local.

## Load into Power BI Desktop

- Home → **Get data** → **Text/CSV** → load `Financials.csv` and `SharesSold.csv`
- Transform data (Power Query):
  - Ensure `year` is **Whole Number**
  - If `Total Shares Sold` imports as text, remove commas/spaces and set to **Whole Number**
  - Set numeric columns to **Decimal Number** (or **Fixed Decimal**) as appropriate

## Model

Create a relationship:
- `Financials[year]` → `SharesSold[year]`

Optional (recommended): add a Year dimension.

```DAX
YearDim =
ADDCOLUMNS(
  CALENDAR(DATE(2002,1,1), DATE(2022,12,31)),
  \"Year\", YEAR([Date])
)
```

Relate `YearDim[Year]` to both fact tables’ `year`.

## Measures (DAX)

Adjust column names to match your CSV headers.

```DAX
Revenue = SUM(Financials[Revenue ($B)])
Earnings = SUM(Financials[Earnings ($B)])
Market Cap = SUM(Financials[Market cap ($B)])
Operating Margin = AVERAGE(Financials[Operating Margin (%)])
Debt = SUM(Financials[Total debt ($B)])
Dividend Yield = AVERAGE(Financials[Dividend Yield (%)])
Shares Outstanding = SUM(Financials[Shares Outstanding ($B)])
Avg High = AVERAGE(SharesSold[Average of High])
Shares Sold = SUM(SharesSold[Total Shares Sold])
```

Year-over-year example:

```DAX
Revenue YoY % =
VAR prev =
  CALCULATE([Revenue], DATEADD(YearDim[Date], -1, YEAR))
RETURN
DIVIDE([Revenue] - prev, prev)
```

## Recommended report pages

### Page 1 — Overview

- **Line chart**: Year vs `Revenue` and `Earnings`
- **Line chart**: Year vs `Market Cap`
- **Cards** (latest year): Revenue, Earnings, Market Cap, Operating Margin, Debt
- **Slicer**: Year

### Page 2 — Profitability & returns

- **Line chart**: Year vs `Operating Margin`
- **Line chart**: Year vs `EPS ($)` + `Dividend Yield`
- **Line chart**: Year vs `Shares Outstanding` (buybacks)

### Page 3 — Leverage & risk

- **Combo chart**: Columns = `Debt`, Line = `Operating Margin`
- **Line chart**: Year vs `Total liabilities ($B)` (if present)
- **Bar chart**: Year vs `Shares Sold` (trading volume)

## Publish

- Home → **Publish** (Power BI Service)
- Pin the main Overview visuals to a dashboard for a one-screen summary.

