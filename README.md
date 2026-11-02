# Retail Sales Data Cleaning with Python and Pandas

## Project Overview

This project demonstrates a beginner-friendly data-cleaning workflow using **Python** and **Pandas** in Jupyter Notebook. It uses a messy retail orders dataset to practise preparing raw business data for further analysis.

The work focuses on identifying and handling missing values, converting columns to suitable data types, removing duplicate records, and standardising text fields.

## Tools and Technologies

- Python
- Pandas
- Jupyter Notebook
- CSV

## Dataset

The dataset contains retail order information, including fields such as:

- `order_id` – order reference
- `order_date` – date the order was placed
- `customer_name` – customer name
- `email` – customer email address
- `phone` – customer phone number
- `product` – product purchased
- `category` – product category
- `quantity` – number of items ordered
- `unit_price` – price per item
- `total_amount` – total order value
- `city` – order/customer location
- `payment_method` – payment method
- `order_status` – order status
- `sales_rep` – sales representative
- `customer_note` – additional order information

The notebook currently loads a CSV named `messy_dataset_3000.csv`. Make sure the CSV is in the same folder as the notebook, or update the filename/path in the `pd.read_csv()` cell to match the actual file you are using.

## Cleaning Tasks Practised

### 1. Loading and inspecting the data

The notebook imports Pandas, reads the CSV file, and inspects the columns and missing-value counts.

```python
import pandas as pd

messy_data = pd.read_csv("messy_dataset_3000.csv")

messy_data.head()
messy_data.columns
messy_data.isnull().sum()
```

### 2. Handling missing values

Text columns such as customer name, product, city, payment method, sales representative, customer note, and email are filled with the label `Not Specified`.

The notebook also converts `quantity` to a numeric type and fills missing quantity values using the column median.

```python
messy_data["quantity"] = pd.to_numeric(
    messy_data["quantity"], errors="coerce"
)

messy_data["quantity"] = messy_data["quantity"].fillna(
    messy_data["quantity"].median()
)
```

### 3. Checking and removing duplicates

The notebook checks for exact duplicate rows and duplicate email values. It then removes duplicates using the email column:

```python
messy_data = messy_data.drop_duplicates(subset=["email"])
```

**Important data-cleaning consideration:** An email address can legitimately appear on multiple orders because one customer may place several orders. Removing every repeated email can therefore delete valid transactions. For a real sales dataset, decide what makes an order unique—often `order_id`—and check repeated IDs before removing records. Use full-row duplicate removal when the goal is to remove only identical records:

```python
messy_data = messy_data.drop_duplicates()
```

### 4. Standardising text

The notebook removes leading and trailing spaces and applies title case to selected text columns.

```python
messy_data["customer_name"] = messy_data["customer_name"].str.strip().str.title()
messy_data["product"] = messy_data["product"].str.strip().str.title()
messy_data["category"] = messy_data["category"].str.strip().str.title()
messy_data["sales_rep"] = messy_data["sales_rep"].str.strip().str.title()
```

## Important Notes and Possible Improvements

This is a learning project, so some cleaning decisions should be reviewed before using the data for business reporting:

- **Phone numbers:** Phone numbers are identifiers, not quantities. Do not calculate their median. Keep them as text and fill missing entries with a label such as `Not Specified` if appropriate.
- **Total amount:** Fill missing `total_amount` values using the total-amount median only if that is an appropriate choice for the analysis. The notebook contains an earlier line that fills this column using the median of `quantity`; that is a column mismatch and should be corrected or removed.
- **Unit price:** Convert messy currency/text values to numeric before calculating a median. Values that cannot be converted become missing values when using `errors="coerce"`. Review those records before filling them.
- **Missing-value labels:** Replacing missing values with `Not Specified` is useful for some text fields, but it is not always appropriate for every column. Keep missing values where the absence itself matters.
- **Dates and numeric columns:** For a more complete cleaning workflow, validate and standardise `order_date`, `unit_price`, and `total_amount`, and check that totals are consistent with quantity multiplied by unit price.
- **Working on a copy:** If the DataFrame was created by filtering another DataFrame, use `.copy()` when creating it to avoid `SettingWithCopyWarning`.

## How to Run the Project

1. Download or clone this repository.
2. Place the retail orders CSV file in the same folder as `clean_messy_data.ipynb`.
3. Install the required library if needed:

   ```bash
   pip install pandas jupyter
   ```

4. Start Jupyter Notebook:

   ```bash
   jupyter notebook
   ```

5. Open `clean_messy_data.ipynb`.
6. Update the CSV filename in the `pd.read_csv()` cell if your file has a different name.
7. Run the notebook cells from top to bottom.

## Skills Demonstrated

- Reading CSV files with Pandas
- Inspecting DataFrames
- Identifying missing values
- Filling missing values
- Converting data types with `pd.to_numeric()`
- Detecting and removing duplicates
- Cleaning text with `.str.strip()` and `.str.title()`
- Checking the results of data-cleaning operations

## Project Files

A typical repository structure is:

```text
retail-data-cleaning/
│
├── clean_messy_data.ipynb
├── messy_dataset_3000.csv
└── README.md
```

## Conclusion

This project provided practical experience cleaning a messy retail dataset with Pandas. It strengthened my understanding of missing values, data types, duplicate records, and text standardisation—important steps in preparing data for analysis.

---

**Author:** Alex Nmesoma
