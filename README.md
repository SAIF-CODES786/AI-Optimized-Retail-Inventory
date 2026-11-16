# AI-Optimized Retail Inventory Management System

A machine-learning project for retail sales forecasting and inventory planning using historical product, stock, sales, category, season, cost, and supplier lead-time data.

## Project contents

- `notebooks/retail_inventory_analysis.ipynb` — cleaned Colab/Jupyter notebook (cell outputs removed to keep the repository lightweight).
- `data/dataset.csv` — dataset used by the notebook.
- `requirements.txt` — Python dependencies.
- `docs/` — project documentation.

## Dataset

The CSV contains 500 records and 10 columns:

| Column | Description |
|---|---|
| date | Record date |
| product_id | Product/SKU identifier |
| category | Product category |
| season | Season label |
| stock_qty | Available stock quantity |
| reorder_level | Reorder threshold |
| daily_sales | Daily units sold |
| unit_cost | Product unit cost |
| supplier_id | Supplier identifier |
| lead_days | Supplier lead time in days |

## Models and evaluation

The notebook compares Linear Regression, Random Forest, and XGBoost using MAE, RMSE, and R².

The saved notebook output reported the following test metrics:

| Model | Test MAE | Test RMSE | Test R² |
|---|---:|---:|---:|
| Linear Regression | 17.452 | 21.439 | -0.702 |
| Random Forest | 14.032 | 16.299 | 0.016 |
| XGBoost | 14.381 | 16.679 | -0.030 |

These are the metrics present in the supplied notebook output, not a claim of production-ready performance. The low/negative test R² values indicate that further feature engineering, validation, and model tuning are needed.

## Run locally

1. Clone this repository:
   ```bash
   git clone https://github.com/SAIF-CODES786/AI-Optimized-Retail-Inventory.git
   cd AI-Optimized-Retail-Inventory
   ```
2. Create and activate a virtual environment (recommended).
3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
4. Open `notebooks/retail_inventory_analysis.ipynb` in Jupyter or Google Colab.

### Google Colab dataset path

The original notebook reads `/content/dataset.csv`. When running in Colab, upload `data/dataset.csv` into the Colab session and keep the filename `dataset.csv` in `/content/`. Alternatively, change the notebook read path to the location where you mount the dataset.

## Current limitations

- The notebook currently depends on a dataset file being available at the expected path.
- Re-run all cells in order in a fresh runtime to reproduce results.
- Model performance needs further validation before real-world inventory decisions are made.

## Author

**Saif Khan**  
GitHub: [@SAIF-CODES786](https://github.com/SAIF-CODES786)
