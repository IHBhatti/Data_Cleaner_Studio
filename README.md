# Data_Cleaner_Studio
# 🧹 Automated Data Cleaning Studio

A Streamlit web app that transforms messy, raw CSV files into clean, analysis-ready datasets — automatically detecting data types, removing duplicates, handling missing values, and treating outliers.
---

## ✨ Features

| Feature | Description |
|---|---|
| 🔍 **Smart Type Detection** | Automatically converts text-stored numbers (`"$50,000"` → `50000.0`), mixed-format dates (`"01/20/2021"`, `"March 5, 2022"` → `datetime64`), and Yes/No values → boolean |
| 🏷️ **Column Standardization** | Converts column names to clean `snake_case` (e.g., `"Employee Name"` → `employee_name`) |
| 🔁 **Duplicate Removal** | Drops exact duplicate rows |
| ❓ **Missing Value Handling** | Median/mean/zero for numeric, mode for categorical, interpolation for dates — configurable in the sidebar |
| 📈 **Outlier Treatment** | IQR-based detection with capping (winsorize), removal, or detect-only modes |
| 📊 **Transparency Report** | Tabbed before/after view showing exactly what changed and why |
| ⬇️ **One-Click Download** | Export the cleaned dataset as CSV |

---

## 🚀 Demo

Upload a messy CSV like this:

| Employee Name | Salary ($) | Join Date | Is Active? |
|---|---|---|---|
| `  Alice  ` | `$50,000` | `2021-01-15` | `Yes` |
| `Bob` | `60,000` | `01/20/2021` | `No` |
| `Charlie` | `(20000)` | `March 5, 2022` | `Y` |

Get back a clean dataset:

| employee_name | salary | join_date | is_active |
|---|---|---|---|
| Alice | 50000.0 | 2021-01-15 | True |
| Bob | 60000.0 | 2021-01-20 | False |
| Charlie | -20000.0 | 2022-03-05 | True |

Plus a full report on every transformation applied.

---

## 🛠️ Installation

```bash
https://github.com/IHBhatti/Data_Cleaner_Studio
cd data-cleaning-studio
pip install -r requirements.txt
```

## ▶️ Usage

```bash
streamlit run app.py
```

The app opens at `http://localhost:8501`. Upload `sample_messy_data.csv` (included) to try it out, or use your own CSV.

---

## 📁 Project Structure

```
data-cleaning-studio/
├── app.py                  # Streamlit UI
├── cleaning_engine.py       # Core cleaning pipeline (pure pandas/numpy, no Streamlit deps)
├── requirements.txt
├── sample_messy_data.csv    # Example messy dataset
└── README.md
```

---

## ⚙️ How It Works

The pipeline runs in this order:

1. **Standardize column names** — strip whitespace, remove special characters, convert to `snake_case`
2. **Normalize text** — trim and collapse whitespace in text columns
3. **Remove duplicates** — drop exact duplicate rows
4. **Detect & convert types** — for each object column, try in order:
   - Numeric (strips `$`, `,`, `%`, handles `(123)` → `-123`)
   - Datetime (handles mixed formats within the same column)
   - Boolean (`Yes/No`, `True/False`, `Y/N`, `1/0`)
   - Category (low-cardinality text)
   - Falls back to plain text
   - *A conversion only applies if ≥80% of values parse successfully*
5. **Handle missing values** — strategy depends on column type; columns >60% missing are dropped
6. **Handle outliers** — IQR method (`Q1 - 1.5×IQR`, `Q3 + 1.5×IQR` by default, adjustable)

## 🤝 Contributing

Issues and pull requests are welcome. If you'd like a custom version of this tool for your own data pipeline, feel free to reach out.


---

⭐ If this project saved you time, consider starring the repo!
