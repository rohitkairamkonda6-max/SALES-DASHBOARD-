# 📊 Sales Analytics & Forecasting Dashboard

A browser-based dashboard for retailers. Upload your sales CSV files and instantly get revenue, profit, trends, top products and a future-sales forecast. No backend, no install, and your data never leaves your device.

**🔗 Live demo:** https://rohitkairamkonda6-max.github.io/sales-dashboard/

<!-- Add a screenshot: save it as screenshot.png in the repo root, then uncomment -->
<!-- ![Dashboard screenshot](screenshot.png) -->

## Why this project
Small retailers often keep sales in Excel or CSV exports and can't easily see which products, cities or months drive revenue, or what next month might look like. This tool turns raw sales records into decisions in seconds.

## Features
- **Multi-file CSV upload:** select one or many files and they merge into one dataset
- **Smart column matching:** recognises names like `payment date`, `sales amount`, `city`, `quantity`; handles comma, semicolon and tab delimiters and day-first dates (`31-01-2026`)
- **KPIs:** revenue, profit, margin, orders, units sold, average order value, month-over-month growth
- **Margin handling:** uses real cost if your CSV has a `cost` column, otherwise estimates profit from a Margin % you enter
- **Filters:** region, category and date range
- **Charts:** monthly revenue and profit, revenue by category, revenue by region
- **Top products table:** units, revenue, profit, margin
- **Sales forecast:** next 3, 6 or 12 months using linear regression or a 3-month moving average, with an R² fit score
- **Clear error messages** when a file can't be read
- Light/dark mode and mobile-friendly layout

## CSV format
Required (names are matched flexibly):

| Data | Accepted column names |
|---|---|
| Date | `date`, `payment date`, `order date`, `invoice date` |
| Product | `product`, `product name`, `item`, `sku` |
| Quantity | `quantity`, `qty`, `units` |
| Revenue | `sales amount`, `amount`, `total`, `revenue` **or** `price` / `unit price` |

Optional: `city`/`region`, `category`, `cost` (per unit), `order number`.

A working example is in [`sample-data/sales-sample.csv`](sample-data/sales-sample.csv).

## How it works
1. **Ingest:** CSV parsed in the browser; headers matched to aliases; dates normalised to `YYYY-MM-DD`.
2. **Calculate:** `revenue = quantity × price` (or the sales amount directly); `profit = revenue − cost` (or `revenue × margin %`).
3. **Aggregate:** rows grouped by month, category, region and product.
4. **Forecast:** a least-squares line is fitted to monthly revenue and extended forward. An incomplete latest month is excluded so it doesn't skew the trend.
5. **Render:** Chart.js draws the charts; KPI cards update live with filters.

## Run locally
No build step needed:
```bash
git clone https://github.com/rohitkairamkonda6-max/sales-dashboard.git
cd sales-dashboard
# open index.html in your browser, or:
python3 -m http.server 8000   # then visit http://localhost:8000
```

## Deploy (GitHub Pages)
Repo → **Settings → Pages** → Source: *Deploy from a branch* → Branch: `main` / `(root)` → Save. Your site goes live at `https://rohitkairamkonda6-max.github.io/sales-dashboard/` within a minute or two.

## Tech stack
HTML, CSS, vanilla JavaScript · [Chart.js](https://www.chartjs.org/) 4.4.1 · GitHub Pages

## Limitations
- Forecasts are trend-based and don't model seasonality (needs 2+ years of data)
- Without a `cost` column, profit is an estimate based on the Margin % input
- Each upload replaces the previous data (select all files in one upload to combine them)

## Roadmap
- [ ] Seasonal forecasting
- [ ] Per-category margins
- [ ] Unique order counting by order number
- [ ] Python/Flask or Node backend with PostgreSQL and login
- [ ] Unit tests for calculation functions

## License
MIT, see [LICENSE](LICENSE).
