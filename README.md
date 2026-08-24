# Sales Data Cleaning & Exploratory Data Analysis (EDA) Pipeline

An end-to-end data processing and exploratory analysis pipeline built in Python to clean, standardize, and analyze raw sales transaction records. The pipeline handles messy real-world data issues—such as mixed date formats, inconsistent text casing, dirty currency strings, negative quantities, and duplicate transactions—before deriving sales KPIs and time-based metrics.

---

## Project Overview

* **Input Data:** `rough_sales_data.csv` (2,000 raw transaction rows × 8 columns)
* **Cleaned Data:** 1,919 deduplicated transaction rows × 14 columns (including 6 derived time and revenue features)
* **Analysis Scope:** Complete sales performance analysis across 1,391 fully-populated sales records from January 2023 through December 2024 

---

## Dataset Schema

### Initial Attributes
* `OrderID`: Unique alphanumeric identifier for each order .
* `OrderDate`: Transaction date stored across inconsistent string formats .
* `Product`: Product name with mixed casing and spacing .
* `Quantity`: Number of units purchased (included negative entry errors) .
* `UnitPrice`: Unit price containing currency symbols (`$`), commas, and text `N/A` .
* `Region`: Sales region containing inconsistent whitespace and casing .
* `SalesRep`: Name of the sales representative .
* `Status`: Order fulfillment status .

### Engineered Features
* `TotalSales`: Computed transaction value (`Quantity` $\times$ `UnitPrice`) .
* `Year`, `Month`, `MonthName`, `Quarter`, `YearMonth`: Extracted temporal dimensions for trend and seasonality analysis .

---

## Data Cleaning & Transformation Pipeline

| Step | Issue Identified | Remediation Applied |
| :--- | :--- | :--- |
| **1. Empty Rows** | 5 completely empty rows in the dataset . | Removed via `df.dropna(how='all')` . |
| **2. Whitespace & Nulls** | Unwanted spaces and text `'nan'`/`'NaN'` strings across object columns . | Applied `.str.strip()` across string fields and normalized string nulls to `np.nan` . |
| **3. Text Normalization** | Inconsistent casing and spacing in `Product`, `Region`, `SalesRep`, and `Status` . | Converted values to Title Case and collapsed multiple whitespace characters via regex . |
| **4. Price Cleaning** | Currency symbols (`$`), commas, and `'N/A'` strings stored in `UnitPrice` . | Stripped non-numeric characters and cast column to . |
| **5. Quantity Correction** | 52 records contained negative quantity values (e.g., `-1.0`) due to data entry errors . | Replaced values with absolute values using `.abs()` . |
| **6. Multi-Format Date Parsing** | Dates formatted inconsistently (e.g., `Oct 18, 2024`, `2024-08-20`, `11-06-2023`, `03/13/2023`) . | Parsed with `dateutil.parser` with explicit regex fallbacks for `DD-MM-YYYY` and `MM/DD/YYYY` . |
| **7. Deduplication** | 76 duplicate `OrderID` occurrences . | Dropped duplicates on `OrderID`, keeping the first valid record . |

---

## Key Business Metrics & Summary

| Metric | Result |
| :--- | :--- |
| **Total Revenue** | $679,906.73  |
| **Total Valid Orders Analyzed** | 1,391  |
| **Total Units Sold** | 2,810  |
| **Average Order Value (AOV)** | $488.79  |
| **Date Range Covered** | Jan 1, 2023 – Dec 31, 2024  |

### Key Findings
* **Top Product by Revenue:** Widget A ($123,722.76), followed by Gadget X ($107,670.34) .
* **Top Region by Revenue:** North ($178,360.36), followed by East ($173,900.03) and South ($169,796.37) .
* **Top Sales Representative:** Bob Smith ($133,869.39; 276 orders), followed by Charlie Lee ($122,349.05) .
* **Order Status Distribution:** Over 64% of orders completed across all regions .

---

## Tech Stack & Dependencies

* **Python 3.10+**
* **Data Processing:** `pandas`, `numpy`, `python-dateutil` 
* **Data Visualization:** `matplotlib`, `seaborn` 
* **Pattern Matching:** `re` 

---
