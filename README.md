# Apple-_Sales-_Analysis

# 📱 iPhone Data Cleaning & Visualization

An end-to-end exploratory data analysis (EDA) project on **62 Apple iPhone listings from Flipkart**. The notebook takes a raw product dataset through inspection, cleaning, feature engineering, and visualization using Python, and ends with a cleaned dataset and a set of insights about pricing, discounts, ratings, and popularity.

---

## 📌 Table of Contents

- [Project Overview](#-project-overview)
- [Dataset](#-dataset)
- [Tools & Libraries](#-tools--libraries)
- [Project Workflow](#-project-workflow)
- [Feature Engineering](#-feature-engineering)
- [Key Insights](#-key-insights)
- [Visualizations](#-visualizations)
- [Project Structure](#-project-structure)
- [How to Run](#-how-to-run)
- [Future Improvements](#-future-improvements)
- [Author](#-author)

---

## 🎯 Project Overview

Raw data is rarely analysis-ready. The goal of this project is to practice the full data-preparation workflow that comes before any modelling:

1. Understand the structure and quality of a real-world e-commerce dataset
2. Detect and handle missing values, duplicates, and logical errors
3. Standardize column names and text fields
4. Engineer new, more useful features
5. Explore the data with statistics and visualizations
6. Export a clean dataset ready for further analysis

---

## 📂 Dataset

| Property | Details |
|---|---|
| **File** | `apple_products.xlsx` |
| **Source** | iPhone listings from Flipkart |
| **Rows** | 62 |
| **Original columns** | 11 |
| **Columns after cleaning & feature engineering** | 16 |

**Original columns**

| Column | Description |
|---|---|
| Product Name | iPhone model with colour and storage |
| Product URL | Link to the product listing |
| Brand | Brand name (Apple) |
| Sale Price | Current selling price (₹) |
| Mrp | Maximum retail price (₹) |
| Discount Percentage | Discount shown on the listing |
| Number Of Ratings | Total number of customer ratings |
| Number Of Reviews | Total number of written reviews |
| Upc | Unique product code |
| Star Rating | Average customer rating |
| Ram | RAM size |

> **Note:** The dataset covers iPhone 8, 8 Plus, XR, XS Max, SE, 11, 11 Pro, 11 Pro Max, 12 Pro, and 12 Pro Max variants.

---

## 🛠 Tools & Libraries

- **Python 3.12**
- **Pandas** – data manipulation and cleaning
- **NumPy** – numerical operations
- **Matplotlib** – base plotting
- **Seaborn** – statistical visualizations
- **Jupyter Notebook** – development environment

---

## 🔄 Project Workflow

### 1. Data Understanding
- Loaded the Excel file and previewed it with `head()`, `tail()`, and `sample()`
- Checked shape, column names, data types, and summary statistics (`info()`, `describe()`)

### 2. Data Quality Checks
- **Missing values:** none found across all 11 columns
- **Duplicates:** checked and removed with `drop_duplicates()` (0 remaining)
- **Logical errors:** verified there were no cases of `sale_price > mrp`, negative prices, star ratings outside 0–5, or discounts above 100%

### 3. Data Cleaning
- Standardized column names: stripped whitespace, lowercased, replaced spaces with underscores (e.g. `Sale Price` → `sale_price`)
- Cleaned the `product_name` text (trimmed whitespace, converted to lowercase)
- Ensured numeric columns were correctly typed using `pd.to_numeric()`
- Added safeguards to fill any missing prices with the median and missing brands with `"Unknown"`

### 4. Outlier Detection
- Used boxplots on `sale_price`, `mrp`, and `star_rating` to inspect the spread and spot extreme values

### 5. Feature Engineering
See the [next section](#-feature-engineering).

### 6. Exploration & Analysis
- Sorting and filtering (e.g. premium phones with high ratings, heavily discounted models)
- `groupby` and `agg` to summarize price and rating statistics
- Rankings: top 10 most expensive, most reviewed, and most rated products

### 7. Visualization
Histograms, boxplots, bar charts, count plots, scatter plots, and a correlation heatmap (details below).

### 8. Export
- Saved the cleaned data as `apple_products_cleaned.csv` and `apple_products_cleaned.xlsx`
- Saved a sale price distribution chart as a high-resolution PNG

---

## ⚙️ Feature Engineering

| New Column | How it was created | Purpose |
|---|---|---|
| `calculated_discount` | `(mrp - sale_price) / mrp × 100` | The actual discount computed from prices |
| `discount_difference` | `discount_percentage - calculated_discount` | Compares the listed discount against the calculated one |
| `price_category` | `pd.cut()` on `sale_price` | Groups phones into **Budget** (≤ ₹30k), **Mid Range** (₹30k–60k), **Premium** (₹60k–1L), **Luxury** (> ₹1L) |
| `ram_gb` | Extracted number from the `ram` column | Numeric RAM for analysis |
| `storage_gb` | Regex extraction from `product_name` | Numeric storage capacity |

---

## 💡 Key Insights

- **Price range:** sale prices run from **₹29,999** (iPhone SE) to **₹1,40,900** (iPhone 12 Pro, 512 GB), with an average of about **₹80,074** and a median of **₹75,900**.
- **Price segments:** 22 phones fall in Mid Range, 20 in Luxury, 18 in Premium, and only 2 in Budget.
- **Discounts:** the average listed discount is roughly **10%**, and the highest is around **29%**. The iPhone SE and a few iPhone 11 Pro variants are the most heavily discounted (above 20%).
- **Listed vs. calculated discount:** listed discounts are always within 1 percentage point of the calculated value, which suggests the platform rounds them down.
- **Ratings:** customer ratings are very high and tightly clustered (4.5 to 4.7, mean ≈ 4.58), so star rating alone does little to separate products.
- **Popularity:** the **iPhone SE** dominates customer engagement, with around 95,900 ratings and 8,100+ reviews on its top listings.
- **Correlations:**
  - `sale_price` and `mrp` are almost perfectly correlated (**0.99**)
  - `sale_price` is negatively correlated with `number_of_ratings` (**−0.70**): cheaper phones attract far more ratings
  - `discount_percentage` is positively correlated with `number_of_ratings` (**+0.68**): higher discounts go with higher popularity
  - `sale_price` and `star_rating` have only a weak positive relationship (**0.30**)

---

## 📊 Visualizations

The notebook includes:

| Chart | What it shows |
|---|---|
| Histogram (Matplotlib & Seaborn with KDE) | Distribution of sale prices |
| Boxplots | Spread and outliers in price, MRP, and ratings |
| Bar chart | Average sale price by brand |
| Count plot | Number of products by RAM |
| Scatter plots | Sale price vs. star rating (also coloured by RAM) |
| Histogram | Distribution of discount percentages |
| Correlation heatmap | Relationships between all numeric features |
| Horizontal bar chart | Top 10 most expensive products |

**Sample output – Sale Price Distribution**

![Sale Price Distribution](images/sale_price_distribution.png)

---

## 📁 Project Structure

```
iPhone-Data-Cleaning-Visualization/
│
├── iPhone_DataCleaning_Visualization.ipynb   # Main analysis notebook
├── apple_products.xlsx                       # Raw dataset
├── apple_products_cleaned.csv                # Cleaned dataset (CSV)
├── apple_products_cleaned.xlsx               # Cleaned dataset (Excel)
├── images/
│   └── sale_price_distribution.png           # Saved visualization
├── requirements.txt
└── README.md
```

---

## 🚀 How to Run

1. **Clone the repository**
   ```bash
   git clone https://github.com/<your-username>/iPhone-Data-Cleaning-Visualization.git
   cd iPhone-Data-Cleaning-Visualization
   ```

2. **Install the dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Launch Jupyter Notebook**
   ```bash
   jupyter notebook
   ```

4. Open `iPhone_DataCleaning_Visualization.ipynb` and run all cells (**Kernel → Restart & Run All**).

**`requirements.txt`**
```
pandas
numpy
matplotlib
seaborn
openpyxl
jupyter
```

---


