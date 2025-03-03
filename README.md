# Real Estate Data Analysis using Dask

## Overview
This project demonstrates how to perform real estate data analysis efficiently using Dask, a parallel computing library in Python. The analysis includes data loading, preprocessing, and visualization of key insights using a dataset containing real estate transaction details.

## Requirements
- Python 3.x
- Pandas
- Dask
- Matplotlib

## Steps

### Step 1: Import Required Libraries
```python
import dask.dataframe as dd
import pandas as pd
import matplotlib.pyplot as plt
from dask.distributed import Client
```

### Step 2: Initialize Dask Client for Performance Monitoring
```python
client = Client()
print(client)
```

### Step 3: Load Dataset
```python
dtype_mapping = {
    'Alley': 'object',
    'PoolQC': 'object'
}
df_pandas = pd.read_csv("train.csv")
df_dask = dd.read_csv("train.csv", assume_missing=True, dtype=dtype_mapping)
print(df_dask.head())
```

### Step 4: Data Preprocessing
- Drop rows with missing values in important columns
- Fill missing values in categorical columns

```python
df_dask['Alley'] = df_dask['Alley'].fillna('None')  
df_dask['PoolQC'] = df_dask['PoolQC'].fillna('None')
cleaned_df = df_dask.compute()
print(cleaned_df.head())
```

### Step 5: Perform Analysis
Calculate the average SalePrice by zoning category:
```python
avg_price_by_zone = cleaned_df.groupby("MSZoning")["SalePrice"].mean()
print(avg_price_by_zone)
```

### Step 6: Generate Insights with Visualization
```python
plt.figure(figsize=(8, 5))
plt.bar(avg_price_by_zone.index, avg_price_by_zone.values, color='skyblue')
plt.xlabel("Zoning Category")
plt.ylabel("Average Sale Price")
plt.title("Average Sale Price by Zoning Category")
plt.xticks(rotation=45)
plt.show()
```

### Step 7: Summary of Insights
```python
print("Key Insights:")
print("1. The dataset contains different zoning categories impacting sale prices.")
print("2. The bar chart illustrates how average sale prices vary by zoning.")
```

### Step 8: Stop the Dask Client
```python
client.close()
print("Dask client stopped.")
```

## Conclusion
This project effectively utilizes Dask for handling large datasets efficiently. By leveraging parallel computing, the analysis is faster and scalable. The insights gained from the dataset provide valuable information on how zoning categories impact real estate pricing.

## Author
[Your Name]

