---
title: Pandas
linktitle: Pandas
toc: true
type: docs
date: "2019-11-27T00:00:00+01:00"
draft: false
menu:
  python:
    parent: Data Analysis
    weight: 1

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---

```py
import pandas as pd

pd.Series([list])                       # create a Series object
pd.DataFrame(dict)                      # create df from dictionary
pd.DataFrame(data=[list], index=None,	# create df from list
	columns=None, dtype=None, copy=False)
```
_In this context, the following abbreviations are valid:_

- `df` &rarr; DataFrame (rows and columns);
- `s` &rarr; Series (rows from a single column).

---
## File I/O

```py
df = pd.read_csv(
      r"relative_path", #can be a web address
      usecols = [0, 1, 5], # columns to be returned from the file
      sep = ' ',
      names = ['column1','column2',...],
      header = None,    # line of the header
      nrows = 5,        # number of rows to read
      skiprows = 1,     # rows to skip from top
      skipfooter = 3,   # rows to skip from bottom
                        ## if a list is provided, it will skip those rows
      index_col = False,            # place the name of the column to be used as index
      skipinitialspace = True,      # skip initial spaces after delimiter
      parse_dates = [0],            # list of ints or column names containing date time format
      infer_datetime_format = True  # infer time format and switch to faster loading (if possible)
      )
```

- `pd.read_json(path)` and `pd.read_excel(path)` &rarr; **import** _.json_ or _.xlsx_ into a df
- `df.to_csv(path)`, `df.to_json(path)` and `df.to_excel(path)` &rarr; **export** to _.csv_, _.json_ or _.xlsx_ file

---
## `df` Data Exploration

### Attributes

- `df.shape` &rarr; return number of rows and columns (tuple).
- `df.columns` &rarr; return the name of each column.
- `df.dtypes` &rarr; return **type** of each column.
- `df.index` &rarr; access the indexes.
      - `len(df.index)` &rarr; **number of row** of the df (fastest method)
- `df.values` &rarr; return the values in the _df_ as an **ndarray.**

### Methods

- `df.info()` &rarr; return useful informations about the dataframe.
- `df.head(n=5)`, `df.tail(n=5)` &rarr; prints the first or last _n_ values.
- `df.describe()` &rarr; return **statistics** on the columns.
- `df['columnName'] = df['columnName'].astype(type)` &rarr; **change the type** of the column [`str` (printed as object), `int64`, `float64`].
- `df[‘columnName’].unique()` &rarr; print the **unique values** of the column.

### Display Options

- `pd.set_option("display.max_columns", n)` &rarr; set the number of columns tha should be displayed when printing the df.
      - `display.max_rows` &rarr; as before but for rows.

---
## Data Selection

- `df.iloc[0:n,:]` &rarr; select rows and columns using index location.
      - `df.iloc[[0, 5, 7], [0, 4]]` &rarr; in general, pass a list of rows and columns to be selected.
- `df.loc[44,:]` &rarr; select rows and columns using labels. If the _index_ column is made by integers, rows selection is similar to `iloc`.
      - `df.loc[[0, 5, 7], ["column_1", "column_2"]]` &rarr; in general, pass a list of rows and columns to be selected.
      - `df.loc[:, "column_1":"column_4"]` &rarr; slicing can be used also in columns labels.
- `df[df["Area"] == "Ireland”]` &rarr; select rows where _Area_ is _Ireland_.

{{% alert warning %}}
When **slicing** is used instead of a list of rows/columns, in this context **the last value is included!**
{{% /alert %}}

### Indexing

- `pd.set_index("column_name", inplace=Flase)` &rarr; set the column to be used as index labels for rows. This can replace the default integer indexing.
- `pd.reset_index(inplace=False)` &rarr; reset the index to be integer numbers.

### Filtering

- `filt = (df["column_name"] == value)` &rarr; return a bool Series where the value depends on if the condition is met or not. _Round parenthesis are used just for reading easiness._ This can be used as a **mask** on the dataframe for rows selection.
      - `filt = (df["column_name_1"] == value_1) & (df["column_name_2"] == value_2)` &rarr; use `&` and `|` to combine conditions in a filter.
      - Use `~` to negate a filter when applying it.
- `filt = df["column_name"].isin(list_of_values)` &rarr; create a filter where _True_ is returned if the value of each row is equal to at least one value of the list.
- `filt = df["column_name"].str.contains("string", na=False)` &rarr; check if each row of that column (made by string objects) contains the requested value, not considering _NaN_ rows.

- `df[filt]` or `df.loc[filt]` &rarr; return only the rows where `filt` is _True_.
      - `df.loc[filt, ["column_1", "column_2"]]` &rarr; the use of `loc` allows to select a subset of the columns when applying the filter.

---
## `df` Manipulation

- `df = df.drop(indexNumber,axis=0)` &rarr; **delete** rows.
- `df.insert(location, columnName, values)` &rarr; **insert** a new column in location.
- `df.shift(n, axis=0)` &rarr; **shift rows** by _n_ (can be negative).
- `df.reindex([list])` &rarr; reassign indexes manually.
- `df.rename(columns={oldName1: newName1, ...}, inplace = True)` &rarr; **rename** columns.
- `df.copy(deep=True)` &rarr; copy the `df` indices and data ([doc](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.copy.html#pandas-dataframe-copy)).
- `df.sort_values(['columnName'], ascending=False)` &rarr; **sort** values of _df_.

### Cleaning Data

- `df.dropna()` &rarr; **remove rows** with NaN.
- `df.fillna(new_value)` &rarr; **replace** missing values.
- `df = df.drop("columnName", axis=1)` &rarr; **delete** a column.
      - Alternatively use `df.drop(columns="columnName", inplace=True)`

---
## Data Analysis

- `df['columnName'].sum()`
- `df['columnName'].mean()`
- `df['columnName'].count()`
- `df['columnName'].median()`
- `df.min()` and `df.max()`
- `df.corr(method=Pearson)` &rarr; compute pairwise correlation of columns ([doc](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.corr.html#pandas-dataframe-corr)) - ([Pearson Correlation Coefficient - Wikipedia](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient)).
- `df['columnName'].rolling(int, min_periods).mean()` &rarr; return a Series with the moving average of the data (`int` is the window size) ([doc](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.rolling.html)):
      - `mean()` can be replaced by whichever needed method
      - `min_periods` &rarr; minimum number of periods necessary to return a non-NaN value
- `df.apply(lambda_function)` &rarr; apply a lambda function to the _df._
- `name = df.groupby('columnName')` &rarr; create a **groupby object**. Then, apply methods to this objects [e.g. like `name.sum()`].
- `pd.to_datetime(data, )` &rarr; **convert to datetime** format ([doc](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.to_datetime.html)).

---
## `df` Merge and Join

[docs](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.merge.html), [tutorial](https://www.shanelynn.ie/merge-join-dataframes-python-pandas-index-1/)

```py
new_df = pd.merge(
      df_1,
      df_2[['use_id', 'column2', 'column3']],
      on='use_id',      # set the common column to merge on
                        ## [no need to be called the same]
      how = 'inner',    # specify merge type
      indicator=True,   # create new column that explain
                        ## if the row was present in both df or just one
      )
```

### Merge types

- **Inner merge** (default): _keeps only the common values_ in both the left and right dataframes for the result.
- **Left merge** or **Right merge**: _keep every row in the left or right dataframe_. If there are missing values in the `on` column of the left/right dataframe, add empty/NaN values in the result.
- **Outer merge**: _returns all the rows from the left dataframe, all the rows from the right dataframe, and matches up rows where possible_, with NaNs elsewhere.

{{% alert warning %}}
**Merging on _float_ can be painful**. Merge on _int_ or _string_.
{{% /alert %}}

---
## Resources

- [Pandas Tutorial](https://youtu.be/ZyhVh-qRZPA) - Corey Shafer.