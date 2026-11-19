# ☕ Cafe Sales: Data Cleaning, EDA and Tableau Dashboard

🇺🇦 [Українська версія](README.uk.md)

An end-to-end analytics project: from a deliberately "dirty" cafe sales file to a clean table, exploratory data analysis (EDA) in Python, and an interactive Tableau Public story with three dashboards.

**Author:** Roman Luja

## Links

- 🗂 **Dataset on Kaggle:** [Cafe Sales — Dirty Data for Cleaning Training](https://www.kaggle.com/datasets/ahmedmohamed2003/cafe-sales-dirty-data-for-cleaning-training)
- 📓 **Notebook in Colab:** [eda_cafe_sales.ipynb](https://colab.research.google.com/github/RomanLuja/cafe_sales/blob/main/notebooks/eda_cafe_sales.ipynb)
- 📊 **Tableau Public:** [Cafe Sales Performance & Revenue Drivers](https://public.tableau.com/shared/JZN6C5S7F)

## Dataset

**Cafe Sales — Dirty Data for Cleaning Training** ([Kaggle](https://www.kaggle.com/datasets/ahmedmohamed2003/cafe-sales-dirty-data-for-cleaning-training)): 10,000 cafe transactions for 2023. The data is synthetic and intentionally corrupted: it contains missing values, the words `ERROR` and `UNKNOWN` instead of real values, and other problems.

| Column | Description |
|---|---|
| Transaction ID | unique sale number |
| Item | product: coffee, tea, cake, etc. |
| Quantity | number of units |
| Price Per Unit | price of one unit, USD |
| Total Spent | sale amount, USD (= quantity × price) |
| Payment Method | how the customer paid |
| Location | in-store or takeaway |
| Transaction Date | date of the sale |

## Repository structure

```
cafe_sales/
├── data/
│   ├── dirty_cafe_sales.csv            # raw data
│   └── cafe_sales_clean.csv            # cleaned data
├── notebooks/
│   ├── eda_cafe_sales.ipynb            # project notebook
│   └── requirements.txt                # Python dependencies
├── tableau/
│   └── dashboard.twbx                  # Tableau Public workbook
├── README.md
└── README.uk.md
```

## What was done

**1. Data cleaning (Python)**
- Column names converted to `snake_case`; `ERROR` and `UNKNOWN` replaced with missing values; types fixed (numbers and dates).
- Numbers restored exactly: `total = quantity × price`, and a fixed price for each item.
- Items restored by unique price. Two prices are ambiguous (3.0: Cake or Juice; 4.0: Sandwich or Smoothie), so those rows were filled randomly in proportion to the known shares and flagged in the `item_guessed` column.
- Missing `payment_method` and `location` filled with `Unknown` instead of the most frequent value, to avoid inventing sales.
- 486 rows (4.9 %) that could not be restored were removed: 9,514 rows remain.
- New columns added: `month`, `month_name`, `weekday`, `weekday_name`, `is_weekend`.

**2. Exploratory analysis (Python)**: top items, payment method and location, sales over time, outliers (IQR rule), correlations.

**3. Dashboards (Tableau Public)**: a story with three dashboards.

## Key findings

- **2023 totals:** 84,868.50 USD revenue, 9,514 transactions, average check 8.92 USD.
- **Top items:** Salad leads in revenue (18,220 USD, 21.5 % of total); Coffee leads in units sold (3,766 of 28,744, 13.1 %). Removing the guessed rows does not change the revenue ranking.
- **Payment method (known values):** Digital Wallet 23.02 %, Cash 23.01 %, Credit Card 22.95 % of revenue. The 0.07 percentage-point gap means there is no clear leader. `Unknown` accounts for 31.02 %.
- **Location:** In-store 30.46 % vs Takeaway 29.91 % of revenue (50.5 % vs 49.5 % among known values). `Unknown` accounts for 39.63 %, so this lead is only indicative.
- **Over time:** monthly revenue is stable (average 7,072.38 USD; best month June, 7,353 USD; lowest February, 6,644 USD, which has only 28 days). No clear seasonal trend.
- **Average check:** 8.93 USD on weekdays vs 8.89 USD on weekends.
- **Outliers:** by the IQR rule the normal range of `total_spent` is up to 24.0 USD. All 259 outliers are valid purchases (5 Salads at 5.0 USD = 25.0 USD), so they are kept.
- **Correlations:** quantity and total 0.70, price and total 0.65, quantity and price 0.01.

**Recommendations:** keep Salad always in stock; record payment method and location for every sale to reduce the share of unknown values.

## Tableau story

| Dashboard | What it shows |
|---|---|
| **1. Executive Summary** | Total revenue, average check, number of transactions; top items by revenue and by units sold |
| **2. Customer Behavior** | Top payment method and location, revenue by month, average check by weekday |
| **3. Revenue Stability** | Daily revenue with a 7-day moving average, outliers analysis (box plots), correlation analysis |

## How to run

```bash
pip install -r notebooks/requirements.txt
```

Then open `notebooks/eda_cafe_sales.ipynb` in Google Colab or Jupyter. The notebook reads the raw data from this repository.

## Tools

Python (pandas 2.2.3, numpy 2.4.6, matplotlib 3.10.0, seaborn 0.13.2), Google Colab, Tableau Public.
