# Global Unemployment Data Analysis

This project performs exploratory data analysis (EDA) on global unemployment data from 1991 to 2021. It demonstrates data cleaning, transformation, aggregation, and analysis using Python (`pandas`, `numpy`, `matplotlib`).  

---

## Project Overview

- **Dataset:** `unemployment_analysis.csv`
- **Objective:** Analyze trends in unemployment across countries over time, clean the data, handle missing values, and calculate total unemployment by country.
- **Key Steps:**
  1. Load dataset
  2. Clean numeric values
  3. Handle missing values
  4. Aggregate total unemployment
  5. Summarize statistics for selected countries

---

## Python Code

```python
# Import libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Load dataset
df = pd.read_csv("unemployment_analysis.csv")

# Preview dataset
print(df.head())

# Remove 'lacs' from data and convert to float
df_numeric = df[df.columns[2:33]].replace('lacs', '', regex=True).astype(float)

# Combine cleaned numeric data with country info
df_clean = df.iloc[:, :2].join(df_numeric)

# Interpolate missing values
df_clean.interpolate(inplace=True)

# Verify null values
print(df_clean.isnull().sum())

# Calculate total unemployment per country
total_unemployment = df_clean.iloc[:, 2:].sum(axis=1).reset_index()
country_info = df_clean.iloc[:, 0].reset_index()
unemployment_summary = total_unemployment.merge(country_info)
unemployment_summary.rename(columns={0: "Unemployed_People"}, inplace=True)

print(unemployment_summary.head())

# Example: descriptive statistics for selected countries
selected_countries = df_clean[df_clean["Country Name"].isin(["Benin", "Bahrain"])]
print(selected_countries.describe())

From this analysis, we can conclude:
1. **Trends Over Time:** Economic and social factors cause variations in unemployment rates across countries.
2. **Actionable Insights:** Policymakers can focus on regions with high unemployment for targeted interventions.
3. **Future Work:** Integrating GDP, labor force data, and interactive dashboards could improve analysis and forecasting.

