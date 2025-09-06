---
title: Introduction to Pandas
layout: default
id: pandas-intro
---

# Introduction to Pandas

Pandas is used to load, explore, clean and analyze data all in Python. We can do data analysis tasks in just few lines of code.

- Read data from various formats into Python as structured tables
- Compute summary, aggregations and group operations over data
- Filter and slice datasets to focus on relevant subsets
- Handle missing data gracefully and merge multiple datasets
- Reshape data for easier analysis and visualization
- Visualize data directly from DataFrames using built-in functions

To use the pandas, we first import the pandas library

``` python
import pandas as pd
```

## Creating the dataframe
We can create the data frame from the dictionary of lists. For example,

```python
import pandas as pd

# Define the data as Python dictionary
data = {
    'Product' : ['A', 'B', 'C', 'D'],
    'Region' : ['North', 'South', 'East', 'West'],
    'Sales' : [150, 200, 140, 170],
    'Profit': [30, 55, 20, 45]
    }
# Create Dataframes from data
df = pd.DataFrame(data)
print(df)
```
```
  Product Region  Sales  Profit
0       A  North    150      30
1       B  South    200      55
2       C   East    140      20
3       D   West    170      45

```
The table shows the records with columns for product name, region, sales and profit. Each column in a DAtaFrame is essentially a pandas series object and the DataFrame is like dictionary of series sharing the same index. By default the index starts with 0

## Reading and Writing data with Pandas
One of the important tasks would be reading and writing data. Pandas supports robust I/O capabilities to read and write data from various sources like CSV, Excel, SQL etc.,

To read a CSV file,
```python
df = pd.read_csv('sales_data.csv')
```

To read excel file,
```python
df = pd.read_excel('file.xlsx')
```
We have other formats like `read_json`, `read_html`, `read_sql`, `read_parquet` etc

Writing to files is also straight forward,
```python
df.to_csv('data.csv',index=False)
```

## Selecting and Filtering data in pandas DataFrame
Now you know how to load data into dataframes either from a file or a dictionary. Let us see how we can filter the data

### Selecting specific portion of data
Select single column of data
```python
df['Product']
```
Select multiple columns of data
```python
df[['Product','Sales']]
```
This shows data in 2 dimensions
