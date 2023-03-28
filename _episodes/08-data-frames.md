---
title: "Pandas DataFrames"
teaching: 15
exercises: 15
questions:
- "How can I do statistical analysis of tabular data?"
objectives:
- "Select individual values from a Pandas dataframe."
- "Select entire rows or entire columns from a dataframe."
- "Select a subset of both rows and columns from a dataframe in a single operation."
- "Select a subset of a dataframe by a single Boolean criterion."
keypoints:
- "Use `DataFrame.iloc[..., ...]` to select values by integer location."
- "Use `:` on its own to mean all columns or all rows."
- "Select multiple columns or rows using `DataFrame.loc` and a named slice."
- "Result of slicing can be used in further operations."
- "Use comparisons to select data based on value."
- "Split apply combine operations"
- "Select values or NaN using a Boolean mask."
- "Datetime indicies for operations on timeseries"
---

## Note about Pandas DataFrames/Series

A [DataFrame][pandas-dataframe] is a collection of [Series][pandas-series];
The DataFrame is the way Pandas represents a table, and Series is the data-structure
Pandas use to represent a column.

Pandas is built on top of the [Numpy][numpy] library, which in practice means that
most of the methods defined for Numpy Arrays apply to Pandas Series/DataFrames.

What makes Pandas so attractive is the powerful interface to access individual records
of the table, proper handling of missing values, and relational-databases operations
between DataFrames.

## Selecting values

To access a value at the position `[i,j]` of a DataFrame, we have two options, depending on
what is the meaning of `i` in use.
Remember that a DataFrame provides an *index* as a way to identify the rows of the table;
a row, then, has a *position* inside the table as well as a *label*, which
uniquely identifies its *entry* in the DataFrame.

## Use `DataFrame.iloc[..., ...]` to select values by their (entry) position

*   Can specify location by numerical index analogously to 2D version of character selection in strings.

~~~
import pandas as pd
data = pd.read_csv("data/Thames_Initiative_2009-2017.csv")
data.iloc[0, 4]
~~~
{: .language-python}
~~~
'7.2'
~~~
{: .output}

## Use `DataFrame.loc[..., ...]` to select values by their (entry) label.

*   Can specify location by row and/or column name.

~~~
data.loc[0, "Site name"]
~~~
{: .language-python}
~~~
'Thame at Wheatley'
~~~
{: .output}

## Use `:` on its own to mean all columns or all rows.

*   Just like Python's usual slicing notation.

~~~
data.loc[:, "Lab pH"]
~~~
{: .language-python}
~~~
0       8.01
1       7.94
2       8.05
3       8.14
4       8.20
        ...
9125    7.80
9126    7.81
9127    7.70
9128    7.66
9129    7.75
Name: Lab pH, Length: 9130, dtype: float64
~~~
{: .output}

*   Would get the same result with `data["Lab pH"]` (without a second index).

## Select multiple columns or rows using `DataFrame.loc` and a named slice.

~~~
data.loc[:, "Site name": "Lab pH"]
~~~
{: .language-python}
~~~
	Site name 	Sampling Date 	Time of sampling 	Temperature (oC) 	Lab pH
0 	Thame at Wheatley 	03/03/2009 	09:25 	7.2 	8.01
1 	Thame at Wheatley 	09/03/2009 	09:40 	6.8 	7.94
2 	Thame at Wheatley 	16/03/2009 	10:00 	9.3 	8.05
3 	Thame at Wheatley 	24/03/2009 	09:45 	7.8 	8.14
4 	Thame at Wheatley 	01/04/2009 	09:46 	8.9 	8.20
... 	... 	... 	... 	... 	...
9125 	Cole at Lynt Bridge 	29/08/2017 	12:15 	17.5 	7.80
9126 	Cole at Lynt Bridge 	04/09/2017 	12:10 	15.4 	7.81
9127 	Cole at Lynt Bridge 	11/09/2017 	13:40 	14.5 	7.70
9128 	Cole at Lynt Bridge 	18/09/2017 	13:40 	12.8 	7.66
9129 	Cole at Lynt Bridge 	25/09/2017 	13:20 	15 	7.75

9130 rows × 5 columns
~~~
{: .output}

In the above code, we discover that **slicing using `loc` is inclusive at both
ends**, which differs from **slicing using `iloc`**, where slicing indicates
everything up to but not including the final index.

And here's selecting a subset of rows as well as columns.

~~~
data.loc[0: 10, "Site name": "Lab pH"]
~~~
{: .language-python}
~~~
 	Site name 	Sampling Date 	Time of sampling 	Temperature (oC) 	Lab pH
0 	Thame at Wheatley 	03/03/2009 	09:25 	7.2 	8.01
1 	Thame at Wheatley 	09/03/2009 	09:40 	6.8 	7.94
2 	Thame at Wheatley 	16/03/2009 	10:00 	9.3 	8.05
3 	Thame at Wheatley 	24/03/2009 	09:45 	7.8 	8.14
4 	Thame at Wheatley 	01/04/2009 	09:46 	8.9 	8.20
5 	Thame at Wheatley 	06/04/2009 	09:48 	11.3 	8.20
6 	Thame at Wheatley 	14/04/2009 	09:10 	11.9 	8.11
7 	Thame at Wheatley 	20/04/2009 	10:00 	11.9 	8.00
8 	Thame at Wheatley 	27/04/2009 	09:40 	12.2 	7.98
9 	Thame at Wheatley 	05/05/2009 	09:25 	12.5 	7.90
10 	Thame at Wheatley 	11/05/2009 	09:44 	14.3 	7.93
~~~
{: .output}


## Result of slicing can be used in further operations.

*   Usually don't just print a slice.
*   All the statistical operators that work on entire dataframes
    work the same way on slices.
*   E.g., calculate max of a slice.

~~~
data.loc[:, "Site name": "Lab pH"].max()
~~~
{: .language-python}
~~~
Site name        Wye at Bourne End
Sampling Date           31/10/2016
Lab pH                        9.36
dtype: object
~~~
{: .output}

~~~
data.loc[:, "Site name": "Lab pH"].min()
~~~
{: .language-python}
~~~
Site name        Cherwell at Hampton Poyle
Sampling Date                   01/02/2010
Lab pH                                6.67
dtype: object
~~~
{: .output}

## Use comparisons to select data based on value.

*   Comparison is applied element by element.
*   Returns a similarly-shaped dataframe of `True` and `False`.

~~~
# Use a subset of data to keep output readable.
subset = data.loc[0: 10, "Lab pH"]
subset

# Which values were greater than 10000 ?
subset > 8
~~~
{: .language-python}
~~~
0     8.01
1     7.94
2     8.05
3     8.14
4     8.20
5     8.20
6     8.11
7     8.00
8     7.98
9     7.90
10    7.93
Name: Lab pH, dtype: float64

0      True
1     False
2      True
3      True
4      True
5      True
6      True
7     False
8     False
9     False
10    False
Name: Lab pH, dtype: bool
~~~
{: .output}

## Select values or NaN using a Boolean mask.

*   A frame full of Booleans is sometimes called a *mask* because of how it can be used.

~~~
mask = subset > 8
subset[mask]
~~~
{: .language-python}
~~~
0    8.01
2    8.05
3    8.14
4    8.20
5    8.20
6    8.11
Name: Lab pH, dtype: float64
~~~
{: .output}

What would the following lines do?

~~~
mask = data["Lab pH"] > data["Lab pH"].quantile(0.999)
basic = data[mask]
basic.loc[:, "Site name": "Lab pH"]
~~~
{: .language-python}
~~~
	Site name 	Sampling Date 	Time of sampling 	Temperature (oC) 	Lab pH
1224 	Ock at Abingdon 	03/05/2016 	14:45 	11.9 	8.59
2869 	The Cut at Paley Street 	08/09/2014 	12:00 	16.3 	9.36
3906 	Thames at Wallingford 	11/07/2011 	16:35 	19.2 	8.69
3942 	Thames at Wallingford 	26/03/2012 	17:18 	13 	8.63
4561 	Thames at Hannington 	01/08/2016 	13:10 	16.8 	8.61
5390 	Kennet at Woolhampton 	01/08/2016 	09:35 	15.3 	8.73
5787 	Enborne at Brimpton 	01/08/2016 	09:50 	15.3 	8.66
5954 	Jubilee River at Datchet 	05/03/2012 	12:31 	9.4 	8.88
6047 	Colne at Staines 	01/08/2016 	13:50 	17.7 	8.79
9058 	Cole at Lynt Bridge 	03/05/2016 	12:10 	11.7 	8.59
~~~
{: .output}

## Group By: split-apply-combine

Pandas vectorizing methods and grouping operations are features that provide users
much flexibility to analyse their data.

~~~
data.loc[:, ["Site name", "Lab pH"]].groupby("Site name").mean()
~~~
{: .language-python}
~~~
 	Lab pH
Site name
Cherwell at Hampton Poyle 	7.938881
Cole at Lynt Bridge 	7.946381
Coln at Whelford 	8.006821
Colne at Staines 	8.121493
Enborne at Brimpton 	7.796158
Evenlode at Cassington Mill 	7.933395
Jubilee River at Datchet 	7.950212
Kennet at Woolhampton 	8.034962
Leach at Lechlade 	7.890348
Lodden at Charvil 	7.843077
Ock at Abingdon 	8.007889
Pang at Tidmarsh 	7.930935
Ray at Islip 	7.696256
Thame at Wheatley 	7.846891
Thames at Hannington 	7.915225
Thames at Newbridge 	8.007239
Thames at Runnymede 	7.956557
Thames at Sonning 	7.953984
Thames at Sonning 	8.021600
Thames at Swinford 	8.019977
Thames at Wallingford 	8.021651
The Cut at Paley Street 	7.558159
Windrush at Newbridge 	8.081605
Wye at Bourne End 	8.058515
~~~
{: .output}

Various other aggregation functions, like sum, median, max, etc. are available

Can you create a list of the maximum flow rates by sampling site?

e.g.
~~~
data[["Site name", "Mean daily flow (m3/s)"]].groupby("Site name").max()
~~~
{: .language-python}

Now explain what each line in the following short program does:
what is in `first`, `second`, etc.?

~~~
first = pd.read_csv("data/Thames_Initiative_2009-2017.csv")
second = first[["Site name", "Lab pH", "Mean daily flow (m3/s)"]]
third = second.groupby("Site name").mean()
fourth = third.sort_values("Mean daily flow (m3/s)")
fourth.to_csv("processed.csv")
~~~
{: .language-python}


## Datetime indicies

Pandas is very adept at manipulating timeseries, but the data must be in the correct format first.

Let's take the date and the time columns, combine them, and create a DatetimeIndex

~~~
data["datetime"] = pd.to_datetime(data["Sampling Date"] + " " + data["Time of sampling"])
temporal = data.set_index("datetime")
temporal.index
~~~
{: .language-python}

~~~
DatetimeIndex(['2009-03-03 09:25:00', '2009-09-03 09:40:00',
               '2009-03-16 10:00:00', '2009-03-24 09:45:00',
               '2009-01-04 09:46:00', '2009-06-04 09:48:00',
               '2009-04-14 09:10:00', '2009-04-20 10:00:00',
               '2009-04-27 09:40:00', '2009-05-05 09:25:00',
               ...
               '2017-07-24 12:30:00', '2017-07-31 12:10:00',
               '2017-07-08 12:00:00', '2017-08-14 12:15:00',
               '2017-08-21 12:10:00', '2017-08-29 12:15:00',
               '2017-04-09 12:10:00', '2017-11-09 13:40:00',
               '2017-09-18 13:40:00', '2017-09-25 13:20:00'],
              dtype='datetime64[ns]', name='datetime', length=9130, freq=None)
~~~
{: .output}

We can now index the data by date as well as all the other methods of indexing!

e.g.
~~~
temporal.loc['2009-03-03 09:25:00', "Lab pH"]
temporal.loc['2009-03-03', "Lab pH"]
temporal.loc['2009', "Lab pH"]
~~~
{: .language-python}

It also paves the way for split-apply-combine on time ranges

~~~
temporal["Lab pH"].resample("1W").mean()
~~~
{: .language-python}

More on plotting in a moment, but here's the data resampled to yearly.

~~~
temporal["Lab pH"].resample("1A").mean().plot()
~~~
{: .language-python}

> ## Many Ways of Access
>
> There are at least two ways of accessing a value or slice of a DataFrame: by name or index.
> However, there are many others. For example, a single column or row can be accessed either as a `DataFrame`
> or a `Series` object.
>
> Suggest different ways of doing the following operations on a DataFrame:
> 1. Access a single column
> 2. Access a single row
> 3. Access an individual DataFrame element
> 4. Access several columns
> 5. Access several rows
> 6. Access a subset of specific rows and columns
> 7. Access a subset of row and column ranges
>
{: .challenge}
>
> > ## Solution
> > 1\. Access a single column:
> > ~~~
> > # by name
> > data["col_name"]   # as a Series
> > data[["col_name"]] # as a DataFrame
> >
> > # by name using .loc
> > data.T.loc["col_name"]  # as a Series
> > data.T.loc[["col_name"]].T  # as a DataFrame
> >
> > # Dot notation (Series)
> > data.col_name
> >
> > # by index (iloc)
> > data.iloc[:, col_index]   # as a Series
> > data.iloc[:, [col_index]] # as a DataFrame
> >
> > # using a mask
> > data.T[data.T.index == "col_name"].T
> > ~~~
> > {: .language-python}
> >
> > 2\. Access a single row:
> > ~~~
> > # by name using .loc
> > data.loc["row_name"] # as a Series
> > data.loc[["row_name"]] # as a DataFrame
> >
> > # by name
> > data.T["row_name"] # as a Series
> > data.T[["row_name"]].T # as a DataFrame
> >
> > # by index
> > data.iloc[row_index]   # as a Series
> > data.iloc[[row_index]]   # as a DataFrame
> >
> > # using mask
> > data[data.index == "row_name"]
> > ~~~
> > {: .language-python}
> >
> > 3\. Access an individual DataFrame element:
> > ~~~
> > # by column/row names
> > data["column_name"]["row_name"]         # as a Series
> >
> > data[["col_name"]].loc["row_name"]  # as a Series
> > data[["col_name"]].loc[["row_name"]]  # as a DataFrame
> >
> > data.loc["row_name"]["col_name"]  # as a value
> > data.loc[["row_name"]]["col_name"]  # as a Series
> > data.loc[["row_name"]][["col_name"]]  # as a DataFrame
> >
> > data.loc["row_name", "col_name"]  # as a value
> > data.loc[["row_name"], "col_name"]  # as a Series. Preserves index. Column name is moved to `.name`.
> > data.loc["row_name", ["col_name"]]  # as a Series. Index is moved to `.name.` Sets index to column name.
> > data.loc[["row_name"], ["col_name"]]  # as a DataFrame (preserves original index and column name)
> >
> > # by column/row names: Dot notation
> > data.col_name.row_name
> >
> > # by column/row indices
> > data.iloc[row_index, col_index] # as a value
> > data.iloc[[row_index], col_index] # as a Series. Preserves index. Column name is moved to `.name`
> > data.iloc[row_index, [col_index]] # as a Series. Index is moved to `.name.` Sets index to column name.
> > data.iloc[[row_index], [col_index]] # as a DataFrame (preserves original index and column name)
> >
> > # column name + row index
> > data["col_name"][row_index]
> > data.col_name[row_index]
> > data["col_name"].iloc[row_index]
> >
> > # column index + row name
> > data.iloc[:, [col_index]].loc["row_name"]  # as a Series
> > data.iloc[:, [col_index]].loc[["row_name"]]  # as a DataFrame
> >
> > # using masks
> > data[data.index == "row_name"].T[data.T.index == "col_name"].T
> > ~~~
> > {: .language-python}
> > 4\. Access several columns:
> > ~~~
> > # by name
> > data[["col1", "col2", "col3"]]
> > data.loc[:, ["col1", "col2", "col3"]]
> >
> > # by index
> > data.iloc[:, [col1_index, col2_index, col3_index]]
> > ~~~
> > {: .language-python}
> > 5\. Access several rows
> > ~~~
> > # by name
> > data.loc[["row1", "row2", "row3"]]
> >
> > # by index
> > data.iloc[[row1_index, row2_index, row3_index]]
> > ~~~
> > {: .language-python}
> > 6\. Access a subset of specific rows and columns
> > ~~~
> > # by names
> > data.loc[["row1", "row2", "row3"], ["col1", "col2", "col3"]]
> >
> > # by indices
> > data.iloc[[row1_index, row2_index, row3_index], [col1_index, col2_index, col3_index]]
> >
> > # column names + row indices
> > data[["col1", "col2", "col3"]].iloc[[row1_index, row2_index, row3_index]]
> >
> > # column indices + row names
> > data.iloc[:, [col1_index, col2_index, col3_index]].loc[["row1", "row2", "row3"]]
> > ~~~
> > {: .language-python}
> > 7\. Access a subset of row and column ranges
> > ~~~
> > # by name
> > data.loc["row1":"row2", "col1":"col2"]
> >
> > # by index
> > data.iloc[row1_index:row2_index, col1_index:col2_index]
> >
> > # column names + row indices
> > data.loc[:, "col1_name":"col2_name"].iloc[row1_index:row2_index]
> >
> > # column indices + row names
> > data.iloc[:, col1_index:col2_index].loc["row1":"row2"]
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

[pandas-dataframe]: https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html
[pandas-series]: https://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.html
[numpy]: http://www.numpy.org/
