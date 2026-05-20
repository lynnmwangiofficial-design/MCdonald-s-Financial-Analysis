# Company M Financial Analysis

**Company M Financial Analysis** uses SQL Server and 2002–2022 financial and stock data to show how the company grew earnings and shareholder value mainly through margins, buybacks, and capital returns—not strong revenue growth.

## Visual overview

```mermaid
flowchart LR
  A[(SQL Server\n[Company M])]
  B[dbo.Financials\nannual fundamentals]
  C[dbo.Shares sold\nyearly OHLC + volume]
  D[Join on Year\n+ YoY deltas]
  E[Exports (CSV)\nExcel / Power BI / notebooks]
  F[Business summary\nstakeholder narrative]

  A --> B
  A --> C
  B --> D
  C --> D
  D --> E
  D --> F
```

```mermaid
xychart-beta
  title "Value creation pattern (directional)"
  x-axis ["Revenue", "Earnings", "Market cap", "Shares outstanding"]
  y-axis "Change (approx.)" -50 --> 1000
  bar ["+51", "+371", "+850", "-43"]
```

## Contents


| Path                                                     | Description                                                                   |
| -------------------------------------------------------- | ----------------------------------------------------------------------------- |
| [`Company_M_All_Queries.sql`](Company_M_All_Queries.sql) | Consolidated T-SQL: joins `Financials` and `Shares sold`, exports, YoY deltas |
| [`docs/BUSINESS_SUMMARY.md`](docs/BUSINESS_SUMMARY.md)   | Business impact narrative for stakeholders                                    |
| [`docs/POWER_BI_VISUALS.md`](docs/POWER_BI_VISUALS.md)   | Power BI steps: model + measures + recommended visuals                        |


## Database schema (expected)

- **Database:** `[Company M]`
- **`dbo.[Financials]`** — annual fundamentals (revenue, earnings, debt, dividends, margins, etc.)
- **`dbo.[Shares sold]`** — yearly OHLC averages and total shares traded

Adjust column names in section 5 of the SQL script if your table uses different naming.

## Usage

1. Open `Company_M_All_Queries.sql` in SQL Server Management Studio (or Azure Data Studio).
2. Connect to your server and ensure `[Company M]` exists with the tables above.
3. Run sections individually (`GO` separates batches).
4. Export grids to CSV for Excel, Power BI, or notebooks.

CSV exports are intentionally **not** committed (see `.gitignore`). Generate your own from SSMS.

## License

Educational / portfolio use. Financial figures in `docs/BUSINESS_SUMMARY.md` reflect public-style McDonald's aggregates used for coursework-style analysis—not investment advice.