# Day 7 — Week 1 Review Notes
**Author:** Iung Seangchanmony (Tyson)  
**Date:** 2026-03-29  
**Topic:** Python & Pandas — 3 Weak Concepts Reviewed and Cleared

---

## Concept 1: `df['col']` vs `df[['col']]` — Series vs DataFrame

### What they return
| Syntax | Returns | Dimensions |
|---|---|---|
| `df['price']` | `pandas.Series` | 1D — a single column |
| `df[['price']]` | `pandas.DataFrame` | 2D — a table with one column |

### How to verify it yourself
```python
import pandas as pd

data = {'price': [10, 20, 30], 'qty': [1, 2, 3]}
df = pd.DataFrame(data)

print(type(df['price']))    # <class 'pandas.core.series.Series'>
print(type(df[['price']]))  # <class 'pandas.core.frame.DataFrame'>
```

### The mental model
```
DataFrame = multiple Series stuck together side by side
```

### Why this matters in real work
- Some Pandas functions only accept a **DataFrame**, not a **Series**
- In **Week 3 (Machine Learning)**, Scikit-learn's `fit()` and `predict()` 
  require features as a DataFrame — passing a Series will throw an error
- **Safe rule:** When in doubt, use `df[['col']]` to stay safe

### Real-world example
```python
# This might break in Scikit-learn
X = df['price']          # Series — risky

# This is safe
X = df[['price']]        # DataFrame — always works
```

---

## Concept 2: `fillna()` — The Correct Pattern

### The full correct pattern
```python
# Option 1 — Reassign (recommended)
df['salary'] = df['salary'].fillna(df['salary'].mean())

# Option 2 — inplace parameter
df['salary'].fillna(df['salary'].mean(), inplace=True)
```

### Why reassignment is needed
`fillna()` **does not modify the original DataFrame** — it returns a new result.  
If you don't save it, the change is lost.

```python
# WRONG — change is lost
df['salary'].fillna(df['salary'].mean())

# RIGHT — change is saved
df['salary'] = df['salary'].fillna(df['salary'].mean())
```

### The general pattern for any column and any fill value
```python
df['col'] = df['col'].fillna(df['col'].mean())    # fill with mean
df['col'] = df['col'].fillna(df['col'].median())  # fill with median
df['col'] = df['col'].fillna('Unknown')            # fill text with string
```

### Why this matters in real work
- In production pipelines, unfilled nulls silently break models
- Always verify after filling: `df['salary'].isnull().sum()` should return `0`

---

## Concept 3: Feature Engineering

### Definition
> **Feature engineering** = creating new columns from existing ones  
> to give your analysis or model more information to work with.

It is **not** a shortcut — it is a deliberate decision to extract meaning  
that wasn't directly available in the raw data.

### Example from Day 6 (Movie Analysis Project)
```python
# 'profit' did not exist in the raw dataset
# We engineered it from two existing columns
df['profit'] = df['revenue'] - df['budget']
```

### More examples of feature engineering
```python
# From a date column
df['year'] = pd.to_datetime(df['date']).dt.year
df['month'] = pd.to_datetime(df['date']).dt.month

# From text
df['name_length'] = df['name'].str.len()

# From combining columns
df['revenue_per_unit'] = df['revenue'] / df['units_sold']
```

### Why this matters in real work
- Raw data rarely contains exactly what a model needs
- Feature engineering is one of the **highest-impact skills** in Data Science
- Better features = better models, even with simple algorithms
- In Week 3, you will engineer features before feeding data into Scikit-learn

---

## Week 1 Review Summary

| Concept | Status | Key Takeaway |
|---|---|---|
| Series vs DataFrame | ✅ Cleared | `df['col']` = Series, `df[['col']]` = DataFrame |
| `fillna()` pattern | ✅ Cleared | Must reassign or use `inplace=True` to save changes |
| Feature engineering | ✅ Cleared | Creating new columns from existing ones |

**Week 1 Review Score:** Started at 2/7 → All 3 weak areas now cleared ✅

---

## Coming Up — Day 8: SQL Basics
- `SELECT`, `WHERE`, `ORDER BY`
- 10 practice queries
- Notes and `.sql` file to be pushed to GitHub
