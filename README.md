# Predictive-Inflation-Model-
Predictive Inflation Model :# Predictive Inflation Model Using Bernoulli-Thermodynamic Analogy

---
 Overview

This notebook presents a symbolic and probabilistic approach to modeling inflation using analogies from thermodynamics, particularly Bernoulli's principle. We treat economic variables as symbolic components in a physically-inspired model and then apply Bayesian inference to fit this model to real data.

---

## 📊 1. Load and Prepare Core Economic Data

```python
import pandas as pd

# Load cleaned dataset
data = pd.read_csv("path_to_your_dataset.csv")  # Replace with actual path

# Inspect the dataset
print(data.columns)
data.head()
```

Expected columns: `['Year', 'GDP', 'Inflation', 'Population']`

---

## 📈 2. Compute GDP Per Capita

```python
data["GDP_per_capita"] = data["GDP"] / data["Population"]
```

---

## 🌐 3. Download M2 Money Supply (Constant LCU)

```python
import wbdata
import datetime

country_code = "USA"  # Change this to match your country
date_range = (datetime.datetime(2000, 1, 1), datetime.datetime(2020, 1, 1))
indicators = {"FM.LBL.MQMY.CN": "Money"}

# Download and clean M2 data
money_df = wbdata.get_dataframe(indicators, country=country_code, data_date=date_range)
money_df = money_df.reset_index()
money_df.rename(columns={"date": "Year", "country": "Country"}, inplace=True)
money_df["Year"] = money_df["Year"].dt.year
```

---

## 🔄 4. Merge Economic Data

```python
# Ensure consistent year and country formatting
data["Year"] = pd.to_numeric(data["Year"], errors="coerce")
data["Country"] = country_code

# Merge M2 with core dataset
data = pd.merge(data, money_df, on=["Year", "Country"], how="left")
```

---

## 📊 5. Calculate Bernoulli-Style Economic Constant

```python
data["Money_per_capita"] = data["Money"] / data["Population"]
data["Bernoulli_Constant"] = data["GDP_per_capita"] + data["Money_per_capita"] - data["Inflation"]
```

---

## 🎨 6. Visualize Bernoulli Constant

```python
import matplotlib.pyplot as plt

plt.figure(figsize=(10, 6))
plt.plot(data["Year"], data["Bernoulli_Constant"], marker='o')
plt.title("Economic Bernoulli Constant Over Time")
plt.xlabel("Year")
plt.ylabel("Bernoulli Constant")
plt.grid(True)
plt.show()
```

---

## 📝 Conclusion

This notebook outlines an innovative analogy between economic modeling and thermodynamics. By introducing symbolic computation and incorporating probabilistic inference (to be added), the project paves the way for a more physically grounded understanding of macroeconomic behavior like inflation.

---

## 🎯 Next Steps

* Add Bayesian model using PyMC
* Apply symbolic math to formalize economic laws
* Extend model to include interest rates or unemployment

---

**Author:** Ahmed Gamaleldin*
**Date:** *2025-05-15*

