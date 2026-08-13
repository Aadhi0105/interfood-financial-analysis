# Interfood Group — Five-Year Financial Analysis & Interactive Model

An independent financial analysis of **Interfood Group**, a global dairy commodity
trading company, built from its publicly available integrated reports. The project
transforms five years of reported financials into a clean, queryable dataset and a
suite of controller-style analytics — covering profitability, working-capital
intensity, economic profit (EPR), credit risk, a three-statement projection, and
interactive scenario and goal-seek modelling.

> **Tech stack:** Python · pandas · SQLite · Plotly · Streamlit · openpyxl · Jupyter

---

## Overview

Interfood operates on a distinctive business model: enormous trading volumes
(1M+ tonnes of dairy per year) at thin margins, with a capital-intensive balance
sheet driven by working capital. Because of this, accounting profit alone is a poor
measure of performance — the company itself judges results on **Economic Profit
Realised (EPR)**, which charges operating profit for the cost of capital employed.

This project is built around that reality. Rather than a generic ratio dashboard, it
focuses on the dynamics that actually drive a commodity trader's performance:
margin protection, working-capital discipline, counterparty (credit) risk, and
capital efficiency. Every analytical choice reflects how a **business controller**
in this environment would look at the numbers.

The work is delivered as a sequence of **narrated Jupyter notebooks** — each a
self-contained chapter with a stated objective, explained code, interpreted results,
and a conclusion — plus an interactive **Streamlit dashboard** for live scenario and
goal-seek analysis.

---

## Objectives

- Turn five years of reported figures into a clean, validated, source-referenced dataset.
- Compute the profitability, liquidity, solvency, and capital-efficiency metrics
  most relevant to a dairy trading business.
- Link each material year-on-year movement to the real-world event that caused it.
- Model counterparty credit risk — the P&L and EPR impact of a receivables default.
- Build a driver-based, articulated three-statement projection.
- Enable interactive scenario / stress-testing and single-variable goal-seek ("what
  revenue, volume, or margin is required to hit a target profit?").

---

## Key Features

| Capability | Description |
|---|---|
| **Historical analysis** | Five-year trends in margin, working capital, EPR, solvency, and cash conversion, with charts annotated by the events that drove each move. |
| **Event–impact mapping** | A structured record linking each year's key developments to the specific financial line they affected, and why. |
| **Credit-risk module** | Generalises a real counterparty failure into a repeatable model: P&L and EPR impact of a receivables default across exposure levels and recovery rates. |
| **Three-statement projection** | A driver-based 3-year forecast where the income statement, balance sheet, and cash flow are articulated and tie out. |
| **Scenario & stress-testing** | Flex drivers (margin, working capital, volume, price) and watch all key outputs — gross income, net income, EPR, solvency, cash conversion — respond dynamically. |
| **Goal-seek (reverse modelling)** | Fix a target (e.g. net income or EPR) and solve for the required revenue, volume, or margin, holding other drivers constant. |
| **Interactive dashboard** | A Streamlit app surfacing all of the above with dynamic Plotly charts. |
| **Excel export** | A formatted, programmatically generated workbook of the data, ratios, and charts. |

---

## Repository Structure

```
interfood-financial-analysis/
├── README.md
├── requirements.txt
├── .gitignore
│
├── data/
│ ├── raw/
│ │ ├── income_statement.csv # 5y income-statement line items
│ │ ├── balance_sheet.csv # 5y balance-sheet line items
│ │ ├── cash_flow.csv # 5y cash-flow items (reported or derived)
│ │ ├── kpis.csv # 5y volume, EPR, solvency, current ratio, etc.
│ │ ├── events.csv # material events → metric affected → impact → source
│ │ ├── assumptions.csv # projection drivers (growth, margin, WC days, tax, WACC)
│ │ └── sources.md # provenance: every figure → report year + page
│ └── interfood.db # generated SQLite database (not tracked)
│
├── notebooks/
│ ├── 01_data_preparation.ipynb # load, validate, clean the raw statements
│ ├── 02_database_build.ipynb # build & populate SQLite; sample queries
│ ├── 03_financial_analysis.ipynb # ratios, trends, common-size, cash conversion
│ ├── 04_credit_risk_analysis.ipynb # counterparty / receivables-default modelling
│ ├── 05_three_statement_projection.ipynb # driver-based 3-year articulated projection
│ ├── 06_scenario_and_goalseek.ipynb # stress-testing & reverse (goal-seek) modelling
│ ├── 07_excel_export.ipynb # generate the formatted .xlsx artefact
│ └── 08_insights_summary.ipynb # consolidated findings write-up
│
├── app/
│ └── dashboard.py # Streamlit interactive dashboard
│
├── outputs/
│ ├── interfood_model.xlsx # generated workbook (not tracked)
│ └── charts/ # generated charts (not tracked)
│
└── docs/
└── insights.md # exported summary for easy viewing
```

---

## Notebooks

The notebooks are designed to be read and run in order — each produces an output the
next one consumes.

**`01_data_preparation.ipynb`**
Loads the manually keyed five-year financials, validates them (types, ranges, no
gaps), cleans and structures the three statements plus KPIs and events, and writes a
trusted dataset for the rest of the pipeline.

**`02_database_build.ipynb`**
Creates the SQLite schema, populates it from the cleaned data, and runs sample
queries to confirm integrity. Establishes a queryable, provenance-tracked store.

**`03_financial_analysis.ipynb`**
The analytical core. Computes and interprets the controller metrics: gross-margin
trend, working-capital intensity, EPR trend, solvency and liquidity trajectory,
common-size statements, and cash conversion. Trend charts are annotated with the
events that caused each inflection.

**`04_credit_risk_analysis.ipynb`**
Models counterparty credit risk by generalising a real supplier default into a
repeatable framework: the P&L and EPR impact of a receivables write-off across a
range of exposure sizes and recovery rates, set against the company's thin margin.

**`05_three_statement_projection.ipynb`**
Builds a driver-based three-year forecast. The income statement, balance sheet, and
cash flow are articulated (net income flows to equity, working-capital changes drive
cash, the balance sheet balances). Projections are explicitly illustrative.

**`06_scenario_and_goalseek.ipynb`**
Two capabilities on top of the projection: (1) scenario / stress-testing — flex
drivers and recompute all key outputs; (2) single-variable goal-seek — fix a target
output and solve for the required input driver.

**`07_excel_export.ipynb`**
Generates a formatted Excel workbook (data, ratios, charts) using openpyxl. No macros
— formatting and native charts only.

**`08_insights_summary.ipynb`**
Distils the key findings into a concise narrative — the story the numbers and events
tell across the five years.

---

## The `data/` Folder

- **`raw/`** holds the project's hand-entered source inputs. Financial figures are
  keyed manually from the integrated reports (rather than scraped) to ensure accuracy
  and to preserve a clear audit trail — every figure's source report and page is
  recorded in `sources.md`.
- **`raw/*.csv`** are version-controlled: they are the foundation of the project and
  are not reproducible from code.
- **`interfood.db`** is *generated* by notebook 02 and is **not** tracked — it can be
  rebuilt at any time from the raw data.

---

## The Dashboard (`app/dashboard.py`)

An interactive Streamlit application with tabbed sections:

- **Overview** — headline KPIs and event-annotated five-year trends.
- **Projection** — the three-year forecast, with assumption sliders that redraw the
  projected statements live.
- **Scenario & Stress-Test** — drag drivers (margin, working capital, volume, price)
  and watch every dependent metric and chart update dynamically.
- **Goal-Seek** — set a target profit or EPR and see the revenue, volume, or margin
  required to achieve it.
- **Credit Risk** — set exposure and recovery assumptions to see the default impact.

Run it with:

```bash
streamlit run app/dashboard.py
```

---

## Setup & Installation

```bash
# 1. Clone the repository
git clone https://github.com/Aadhi0105/interfood-financial-analysis.git
cd interfood-financial-analysis

# 2. Create and activate a virtual environment
python3 -m venv venv
source venv/bin/activate        # macOS / Linux
# venv\Scripts\activate         # Windows

# 3. Install dependencies
pip install -r requirements.txt

# 4. Run the notebooks in order (01 → 08), or launch the dashboard
jupyter notebook
# or
streamlit run app/dashboard.py
```

---

## Methodology Notes & Assumptions

- **Data source:** all financial figures are drawn from Interfood Group's publicly
  available integrated reports and entered manually.
- **EPR:** Interfood does not publish its exact cost-of-capital rate, so the EPR
  computations here apply a stated, illustrative assumption; they demonstrate the
  *logic* of economic profit rather than reproduce the company's internal figure.
- **Projections & scenarios:** built on five years of history, all forward-looking
  outputs are directional and illustrative, not statistical forecasts.
- **Goal-seek:** solves for a single free variable at a time, holding others constant,
  to keep the reverse solution well-defined.

---

## Disclaimer

This is an independent, educational portfolio project. It is **not affiliated with,
endorsed by, or produced on behalf of Interfood Group**. All figures are sourced from
publicly available reports and may contain transcription approximations. Forward-
looking projections, scenarios, and credit-risk models are illustrative analytical
exercises based on public data and stated assumptions — they are not forecasts,
valuations, or assessments of the company's actual financial position or
creditworthiness.
