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
To select the data from specific row, the pandas provides `.loc` indexer. So `df.loc[2]` indicates the index with label 2. To return rows from 1 thorugh 3 inclusive we can do `df.loc[1:3]`. and we can also specify rows and columns together `df.loc[1:3, ['Product','Profit']]`

If we would like to use integer positions instead of labels we can use `.iloc` indexer. For example, `df.iloc[0:3]` gives rows at positions 0,1,2 exclusive of 3. And to fetch both row and column indices we can use `df.iloc[[0,2,3],[1,3]] ` would fetch a Dtaframe of specific row indices and column indices.

We can also filter by boolean indexing where we can put a condition in the indexing brackets to filter rows. `df[df['Sales'] > 160]` or `df[(df['Region'] == 'North') & (df['Sales'] > 150)]`

## Data Cleaning and handling missing data
The data can have missing values, duplicates or inconsistent formatting. Pandas has tools to fix these issues
#### Identifying missing values
Missing data in pandas is represented as `NaN` or `None`. We can find missing values using `df.isNull()` which returns True if data is null. We can chain this with `sum()` to count nulls in each column like `df.isnull().sum()`
#### Dropping missing data
If missing data is not too prevelant and we no need those rows we can drop them. `df.dropna()` removes any rows with at least one missing value

- `df.dropna(how='all')` will drop only rows where all values are missing
- `df.dropna(axis=1)` will drop entire columns that have any missing values
- `df.dropna(subset=['Col1','Col2'])` will drop rows that have missing values in specified subset of columns only
By default, `dropna()` returns a new DataFrame and leaves the original intact. If we have to modify in place, we can use `df.dropna(inplace=True)`

#### Filling missing data
In some cases, dropping data is not feasible and we might have to fill some data. we can use `df.fillna(value)` to replace NaNs with a specified `value`

```python
df['Age'].fillna(value=df['Age'].mean(), inplace=True)
```
- `df.fillna(method='ffill') - will forward propogate the last valid value downwards to fill NaNs
- `df.fillna(method='bfill') - will back propogate the last valid value upwards to fill NaNs or gaps

#### Removing Duplicates
Duplicates can be removed using `df.drop_duplicates()` we can also specify subset of columns if we consider duplicates by certain fields only.

#### Renaming columns
We can use `df.rename()` to rename column labels or index labels
```python
df.rename(columns={'Profit':'NetProfit'}, inplace=True)
```
#### Type Conversions
We might want to change the data types of the columns there are methods like `pd.to_datetime(df['Date'])` to convert text to datetime objects or `df['Amount'].astype(float)` to ensure a column is float type.

## Merging an combining data sets
We might have to combine multiple sources. Pandas support various ways to merge or concatenate DataFrames similar to SQL joins

#### Concatenation
We can concatenate 2 dataframes using `pd.concat([df1,df2])` It will append them one after the other by default but if there is conflict with indices, we might have to ignore the original index `pd.concat([df1,df2], ignore_index=True)`. We can also concatenate horizontally by specifying `axis=1` as long as indices align.

#### Merging/ Joining
Pandas has a function called `pd.merge()` to preform database-like joins between two DataFrames. 
```python
df = pd.merge(df_customers, df_orders, on='CustomerID', how='inner')
```
This will perform inner join on CustomerID key 

#### Join Shorthand
If the key for merging is the index of one or both DataFrames we might use df1.join(df2) which by default joins on indices.

## Working with Dates and Times
Time series data is very common and pandas extensively support handling these dates, times and time-indexed data.

#### Parsing dates
For parsing the date columns using 'parse_dates` in `read_csv` or `read_excel` or convert using `to_datetime()` function

#### Date as index
It is always useful to set the datetime as index mainly for time series analysis
```
df['Date'] = pd.to_datetime(df['Date'])
df.set_index('Date', inplace=True)
```
We can also select the ranges with date strings 
```
df['2025-01']        # all data in January 2025
df['2025-06-01':'2025-06-30']  # data for the month of June 2025
```
This is called date slicing

#### Resampling
If we have time series and want to change the frequency like daily data and monthly averages. we can use `df.resample()`
```
monthly_avg = df.resample('M').mean()
```
This groups the data by each calendar month ('M' frequency) and computes the mean of each month

#### Date Range Generation
Pandas can generate sequences of dates with `pd.date_range()`
```
dates = pd.date_range(start='2025-01-01', end='2025-01-10', freq='D')
```
 You could use this to reindex your DataFrame to include all days, then fill missing ones with `fillna`.

Pandas also support time zones and period objects.
