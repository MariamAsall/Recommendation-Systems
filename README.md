## Lifestyle Recommendations (Diet Plan) System

## Overview

This project builds a **Lifestyle Recommendation System** that suggests food items or diet plans based on nutritional content. It uses a real-world food composition dataset and applies data cleaning, outlier handling, and exploratory analysis to prepare the data for future recommendation tasks.

---

## Dataset

**Source:**  
- The dataset is a CSV file named `nutrients_csvfile.csv`.

**Columns:**
- `Food`: Name of the food item
- `Measure`: Serving size/unit
- `Grams`: Weight in grams
- `Calories`: Calories per serving
- `Protein`: Protein (g)
- `Fat`: Total fat (g)
- `Sat.Fat`: Saturated fat (g)
- `Fiber`: Dietary fiber (g)
- `Carbs`: Carbohydrates (g)
- `Category`: Food group/category (e.g., Dairy products, Vegetables)

---

## Steps and Explanations

### 1. **Import Libraries**

The following Python libraries are used:
- `numpy`, `pandas`: Data manipulation
- `sklearn`: Imputation, metrics
- `matplotlib`, `seaborn`: Visualization

### 2. **Load and Inspect Data**

- The dataset is loaded using `pd.read_csv()`.
- `data.head()` and `data.info()` are used to preview data and check types and missing values.

### 3. **Convert Data Types**

- Nutritional columns (`Calories`, `Protein`, `Fat`, `Sat.Fat`, `Fiber`, `Carbs`) are converted to numeric types using `pd.to_numeric(errors='coerce')` to handle non-numeric values (e.g., 't').

### 4. **Handle Missing Values**

- Missing values in numeric columns are imputed using the **mean** with `SimpleImputer(strategy='mean')`.
- After imputation, `data.isnull().sum()` should show zero missing values.

### 5. **Remove Duplicates**

- The code checks for duplicated rows with `data.duplicated().sum()` and removes them if any.

### 6. **Exploratory Data Analysis**

- `data.describe()` provides summary statistics for each numeric column.
- Boxplots are generated for each nutrient to visualize distributions and spot outliers.

### 7. **Outlier Detection and Handling**

- The **IQR (Interquartile Range) method** is used to detect outliers in each numeric column.
- Outliers can be:
  - **Capped** (replaced with the nearest bound) or
  - **Removed** (rows dropped).
- The code demonstrates capping by default.

### 8. **Special Data Cleaning**

- Negative values in the `Protein` column are replaced with the median protein value.
- String values like 't' are replaced with 0.

### 9. **Save Cleaned Data**

- The cleaned and preprocessed dataset is saved as `cleaned_dataset.csv` for downstream use.

### 10. **Visualization**

- Boxplots for each nutrient after cleaning are generated to confirm outlier handling was effective.

---

## Usage

1. **Prepare the Dataset:**  
   Place `nutrients_csvfile.csv` in your working directory.

2. **Run the Notebook:**  
   Execute all cells in order to clean and preprocess the data.

3. **Output:**  
   - Cleaned data is saved as `cleaned_dataset.csv`.
   - Visualizations help you understand the data distribution post-cleaning.

---

## Notes

- The current code focuses on **data cleaning and preprocessing**.  
- The actual recommendation logic (e.g., content-based or collaborative filtering) is not included in the provided code, but the cleaned data is ready for such modeling.
- The system is robust to missing values, outliers, and data type inconsistencies.

---

## Example: Outlier Handling Function

```python
def detect_outliers_iqr(data, column):
    Q1 = data[column].quantile(0.25)
    Q3 = data[column].quantile(0.75)
    IQR = Q3 - Q1
    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR
    outliers = data[(data[column]  upper_bound)]
    return outliers, lower_bound, upper_bound

def handle_outliers(data, column, method='cap'):
    _, lower_bound, upper_bound = detect_outliers_iqr(data, column)
    if method == 'remove':
        data = data[(data[column] >= lower_bound) & (data[column]  upper_bound, upper_bound, data[column])
    return data
```

---

## Troubleshooting

- **FileNotFoundError:** Ensure `nutrients_csvfile.csv` is in the correct directory.
- **ImportError:** Install missing libraries with `pip install pandas numpy scikit-learn matplotlib seaborn`.
- **Data Type Issues:** Non-numeric entries are handled by `errors='coerce'` and replaced with the mean.

---
