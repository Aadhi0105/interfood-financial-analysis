# Data Sources & Provenance

All financial figures in `summary_series.csv` are sourced from Interfood Group's
published Integrated Reports. Figures are entered manually and cross-checked against
the source to preserve an audit trail.

## Primary source

**Interfood Integrated Report 2025** — "Consolidated figures" master table
(Performance section), which presents a **current, restated view of 2013–2025** in a
single table. Because it restates all prior years on a consistent basis, it is used
as the authoritative source for the full 2017–2025 summary series, in preference to
stitching together each year's individual report (which would mix pre- and
post-restatement figures).

- Units in source: **EUR millions** (P&L, balance sheet, cash flow); stored in
  `summary_series.csv` as **EUR '000** (× 1,000). Ratios, %, FTE (count), volume (MT
  '000 → stored as MT '000), and cash-conversion-cycle (days) stored in native units.

## Statement-level detail (for later notebooks)

- **Interfood Integrated Report 2021**, financial statements (pp. 77–80): full
  consolidated balance sheet, P&L, and cash flow for FY2021 (with FY2020 restated).
  Used later for the three-statement line-item detail (notebook 05).

## Key definitions & comparability caveats

These are **not data errors** — they are real breaks in comparability disclosed by
Interfood, and are carried through the analysis:

1. **P&L reclassification as of 2020.** The classification between *gross margin*
   (gross operating income) and *operational expenses* changed in 2020, shifting
   amounts between the two. Pre-2020 vs post-2020 gross margin and opex are therefore
   **not strictly comparable**. (Source: 2025 report table footnote.)

2. **Cash-flow reclassification as of 2023.** Movements in borrowings from credit
   institutions are **no longer classified as financing cash flows** from 2023 onward.
   The operating/financing cash-flow split has a **methodology break at 2023**.
   (Source: 2025 report table footnote.)

3. **"Net result" = net income.** In the report's five-year table, the "Net margin"
   row is the *net operating result*, which is distinct from the "Net result" row
   (net income). `net_result` in the dataset uses the **Net result** row.

4. **Financial income/expenses exclude the E-Piim write-off.** The €14.3m write-off
   related to AS E-Piim Tootmine (2025) is excluded from the financial income/expenses
   line as presented; it flows through net result. (Source: 2025 report table footnote.)

5. **FTE = Average FTE.** The FTE series uses the report's restated "Average FTE
   (number)" row, which differs from year-end headcount figures quoted elsewhere in
   individual reports.

## Metric-level notes

| metric | source row | unit stored | notes |
|---|---|---|---|
| sales | Sales | EUR_000 | net turnover |
| gross_operating_income | Gross margin* | EUR_000 | classification break at 2020 |
| operational_expenses | Operational expenses | EUR_000 | classification break at 2020 |
| net_result | Net result | EUR_000 | = net income (not "Net margin") |
| total_assets | Total assets | EUR_000 | |
| equity | Equity (at year-end) | EUR_000 | |
| net_working_capital | Net working capital (at year-end) | EUR_000 | |
| operating_cash_flow | Operating cash flow | EUR_000 | methodology break at 2023 |
| investing_cash_flow | Investing cash flow | EUR_000 | |
| financing_cash_flow | Financing cash flow | EUR_000 | methodology break at 2023 |
| solvency_pct | Solvency (%) | pct | equity / total assets |
| debt_to_equity | Debt-to-equity | ratio | |
| current_ratio | Current ratio | ratio | |
| fte | Average FTE (number) | count | average, not year-end |
| cash_conversion_cycle | Cash conversion cycle (days) | days | avg duration over year |
| volume_mt | Metric Tonnes sold (MT '000) | MT | stored in MT '000 |
| epr | Economic Profit Realised | EUR_000 | narrative-only; sparse |

## EPR (Economic Profit Realised)

Not present in the five-year summary table. It is discussed in the **narrative** of the
2024 and 2025 reports (operating profit less a charge for capital employed). To be
populated separately from report text where disclosed, and clearly flagged as
management's own metric with an illustrative cost-of-capital assumption where the exact
figure is not given.