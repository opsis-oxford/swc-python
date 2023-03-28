---
title: "Reading Tabular Data into DataFrames"
teaching: 10
exercises: 10
questions:
- "How can I read tabular data?"
objectives:
- "Import the Pandas library."
- "Use Pandas to load a simple CSV data set."
- "Get some basic information about a Pandas DataFrame."
keypoints:
- "Use the Pandas library to get basic statistics out of tabular data."
- "Use `DataFrame.info` to find out more about a dataframe."
- "The `DataFrame.columns` variable stores information about the dataframe's columns."
- "Use `DataFrame.describe` to get summary statistics about data."
---
## Use the Pandas library to do statistics on tabular data.

*   [Pandas](https://pandas.pydata.org/) is a widely-used Python library for statistics, particularly on tabular data.
*   Borrows many features from R's dataframes.
    *   A 2-dimensional table whose columns have names
        and potentially have different data types.
*   Load it with `import pandas as pd`. The alias pd is commonly used for Pandas.
*   Read a Comma Separated Values (CSV) data file with `pd.read_csv`.
    *   Argument is the name of the file to be read.
    *   Assign result to a variable to store the data that was read.
*   Extended as geopandas for geographic data

~~~
import pandas as pd

data = pd.read_csv('data/Thames_Initiative_2009-2017.csv')
print(data)
~~~
{: .language-python}
~~~
     Thames Initiative site code            Site name Sampling Date  \
0                            TC1    Thame at Wheatley    03/03/2009
1                            TC1    Thame at Wheatley    09/03/2009
2                            TC1    Thame at Wheatley    16/03/2009
3                            TC1    Thame at Wheatley    24/03/2009
4                            TC1    Thame at Wheatley    01/04/2009
...                          ...                  ...           ...
9125                         TC9  Cole at Lynt Bridge    29/08/2017
9126                         TC9  Cole at Lynt Bridge    04/09/2017
9127                         TC9  Cole at Lynt Bridge    11/09/2017
9128                         TC9  Cole at Lynt Bridge    18/09/2017
9129                         TC9  Cole at Lynt Bridge    25/09/2017

     Time of sampling Temperature (oC)  Lab pH Gran alkalinity u eq/L  \
0               09:25              7.2    8.01                   4915
1               09:40              6.8    7.94                   5637
2               10:00              9.3    8.05                   5393
3               09:45              7.8    8.14                   5351
4               09:46              8.9    8.20                   5129
...               ...              ...     ...                    ...
9125            12:15             17.5    7.80                   4071
9126            12:10             15.4    7.81                   4429
9127            13:40             14.5    7.70                   2579
9128            13:40             12.8    7.66                   3849
9129            13:20               15    7.75                   4337

     Suspended solids mg/L Soluble reactive phosphorus (ug/L)  \
0                      7.7                                351
1                      7.5                                280
2                      5.3                                322
3                        6                                312
4                      4.4                                408
...                    ...                                ...
9125                 45.51                                319
9126                 38.21                                394
9127                 36.51                                315
9128                 28.34                                329
9129                 40.27                                393

     Total dissolved phosphorus (ug/L)  ... Dissolved Cu (ug/l)  \
0                                  375  ...                 NaN
1                                  292  ...                 NaN
2                                  364  ...                 NaN
3                                  336  ...                 NaN
4                                  442  ...                 NaN
...                                ...  ...                 ...
9125                               336  ...                 NaN
9126                               427  ...                 NaN
9127                               348  ...                 NaN
9128                               324  ...                 NaN
9129                               414  ...                 NaN

     Total Na (mg/l) Total Ca (mg/l) Total Mg (mg/l) Total B (ug/l)  \
0                NaN             NaN             NaN            NaN
1                NaN             NaN             NaN            NaN
2                NaN             NaN             NaN            NaN
3                NaN             NaN             NaN            NaN
4                NaN             NaN             NaN            NaN
...              ...             ...             ...            ...
9125             NaN             NaN             NaN            NaN
9126             NaN             NaN             NaN            NaN
9127             NaN             NaN             NaN            NaN
9128             NaN             NaN             NaN            NaN
9129             NaN             NaN             NaN            NaN

      Total Fe (ug/l) Total Mn (ug/l) Total Zn (ug/l)  Total Cu (ug/l)  \
0                 NaN             NaN             NaN              NaN
1                 NaN             NaN             NaN              NaN
2                 NaN             NaN             NaN              NaN
3                 NaN             NaN             NaN              NaN
4                 NaN             NaN             NaN              NaN
...               ...             ...             ...              ...
9125              NaN             NaN             NaN              NaN
9126              NaN             NaN             NaN              NaN
9127              NaN             NaN             NaN              NaN
9128              NaN             NaN             NaN              NaN
9129              NaN             NaN             NaN              NaN

      Mean daily flow (m3/s)
0                      3.580
1                      4.000
2                      2.300
3                      1.960
4                      1.480
...                      ...
9125                   0.120
9126                   0.183
9127                   0.414
9128                   0.198
9129                   0.452
~~~
{: .output}

*   The columns in a dataframe are the observed variables, and the rows are the observations.
*   Pandas uses backslash `\` to show wrapped lines when output is too wide to fit the screen.

> ## File Not Found
>
> Our lessons store their data files in a `data` sub-directory,
> which is why the path to the file is `data/Thames_Initiative_2009-2017.csv`.
> If you forget to include `data/`,
> or if you include it but your copy of the file is somewhere else,
> you will get a [runtime error]({{ page.root }}/04-built-in/#runtime-error)
> that ends with a line like this:
>
> ~~~
> FileNotFoundError: [Errno 2] No such file or directory: 'data/Thames_Initiative_2009-2017.csv'
> ~~~
> {: .error}
{: .callout}

## Use the `DataFrame.info()` method to find out more about a dataframe.

~~~
data.info()
~~~
{: .language-python}
~~~
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 9130 entries, 0 to 9129
Data columns (total 42 columns):
 #   Column                                 Non-Null Count  Dtype
---  ------                                 --------------  -----
 0   Thames Initiative site code            9130 non-null   object
 1   Site name                              9130 non-null   object
 2   Sampling Date                          9130 non-null   object
 3   Time of sampling                       9103 non-null   object
 4   Temperature (oC)                       8968 non-null   object
 5   Lab pH                                 9074 non-null   float64
 6   Gran alkalinity u eq/L                 9074 non-null   object
 7   Suspended solids mg/L                  8618 non-null   object
 8   Soluble reactive phosphorus (ug/L)     9081 non-null   object
 9   Total dissolved phosphorus (ug/L)      9102 non-null   object
 10  Total phosphorus (ug/L)                9103 non-null   object
 11  Dissolved ammonium (NH4) (mg/l)        9059 non-null   object
 12  Dissolved reactive silicon (mg Si/L)   9107 non-null   object
 13  Chlorophyll-a (ug/L)                   9079 non-null   object
 14  Dissolved fluoride (mg F/L)            9103 non-null   object
 15  Dissolved chloride (mg Cl/L)           9102 non-null   float64
 16  Dissolved nitrite (mg NO2/L)           9103 non-null   object
 17  Dissolved nitrate (NO3)                9103 non-null   object
 18  Dissolved sulphate (mg SO4/L)          9103 non-null   float64
 19  Total dissolved nitrogen (mg N/L)      9103 non-null   float64
 20  Dissolved organic carbon (mg/L)        8045 non-null   float64
 21  Field pH                               4757 non-null   float64
 22  Conductivity (uS/cm)                   4756 non-null   float64
 23  Redox potential (Eh) (mV)              4763 non-null   object
 24  Dissolved B (ppb)                      8096 non-null   object
 25  Dissolved Ca (ppm)                     8096 non-null   float64
 26  Dissolved Fe (ug/l))                   6661 non-null   object
 27  Dissolved K (mg/l)                     8096 non-null   float64
 28  Dissolved Mg (mg/l)                    8096 non-null   float64
 29  Dissolved Na (mg/l)                    8096 non-null   float64
 30  Dissolved Mn (ug/l)                    6418 non-null   float64
 31  Dissolved Zn (ug/l)                    6418 non-null   float64
 32  Dissolved Cu (ug/l)                    6418 non-null   float64
 33  Total Na (mg/l)                        3933 non-null   float64
 34  Total Ca (mg/l)                        3933 non-null   float64
 35  Total Mg (mg/l)                        3933 non-null   float64
 36  Total B (ug/l)                         3933 non-null   float64
 37  Total Fe (ug/l)                        3933 non-null   float64
 38  Total Mn (ug/l)                        3933 non-null   float64
 39  Total Zn (ug/l)                        3933 non-null   float64
 40  Total Cu (ug/l)                        3933 non-null   float64
 41  Mean daily flow (m3/s)                 8884 non-null   float64
dtypes: float64(23), object(19)
memory usage: 2.9+ MB
~~~
{: .output}

*   This is a `DataFrame`
*   9130 rows, each a set of water quality samples
*   41 columns, mixture of float64 and object types
    *   We will talk later about null values, which are used to represent missing observations.
*   Uses 2,900,000 bytes of memory.

## The `DataFrame.columns` variable stores information about the dataframe's columns.

*   Note that this is data, *not* a method.  (It doesn't have parentheses.)
    *   Like `math.pi`.
    *   So do not use `()` to try to call it.

~~~
print(data.columns)
~~~
{: .language-python}
~~~
Index(['Thames Initiative site code', 'Site name', 'Sampling Date',
       'Time of sampling', 'Temperature (oC)', 'Lab pH',
       'Gran alkalinity u eq/L', 'Suspended solids mg/L',
       'Soluble reactive phosphorus (ug/L)',
       'Total dissolved phosphorus (ug/L)', 'Total phosphorus (ug/L)',
       'Dissolved ammonium (NH4) (mg/l)',
       'Dissolved reactive silicon (mg Si/L) ', 'Chlorophyll-a (ug/L)',
       'Dissolved fluoride (mg F/L)', 'Dissolved chloride (mg Cl/L)',
       'Dissolved nitrite (mg NO2/L)', 'Dissolved nitrate (NO3)',
       'Dissolved sulphate (mg SO4/L)', 'Total dissolved nitrogen (mg N/L)',
       'Dissolved organic carbon (mg/L)', 'Field pH', 'Conductivity (uS/cm)',
       'Redox potential (Eh) (mV)', 'Dissolved B (ppb)', 'Dissolved Ca (ppm)',
       'Dissolved Fe (ug/l))', 'Dissolved K (mg/l)', 'Dissolved Mg (mg/l)',
       'Dissolved Na (mg/l)', 'Dissolved Mn (ug/l)', 'Dissolved Zn (ug/l)',
       'Dissolved Cu (ug/l)', 'Total Na (mg/l)', 'Total Ca (mg/l)',
       'Total Mg (mg/l)', 'Total B (ug/l)', 'Total Fe (ug/l)',
       'Total Mn (ug/l)', 'Total Zn (ug/l)', 'Total Cu (ug/l)',
       'Mean daily flow (m3/s)'],
      dtype='object')
~~~
{: .output}

## Use `DataFrame.describe()` to get summary statistics about data.

`DataFrame.describe()` gets the summary statistics of only the columns that have numerical data.
All other columns are ignored, unless you use the argument `include='all'`.
~~~
data.describe()
~~~
{: .language-python}
~~~
 	Lab pH 	Dissolved chloride (mg Cl/L) 	Dissolved sulphate (mg SO4/L) 	Total dissolved nitrogen (mg N/L) 	Dissolved organic carbon (mg/L) 	Field pH 	Conductivity (uS/cm) 	Dissolved Ca (ppm) 	Dissolved K (mg/l) 	Dissolved Mg (mg/l) 	... 	Dissolved Cu (ug/l) 	Total Na (mg/l) 	Total Ca (mg/l) 	Total Mg (mg/l) 	Total B (ug/l) 	Total Fe (ug/l) 	Total Mn (ug/l) 	Total Zn (ug/l) 	Total Cu (ug/l) 	Mean daily flow (m3/s)
count 	9074.000000 	9102.000000 	9103.000000 	9103.000000 	8045.000000 	4757.000000 	4756.000000 	8096.000000 	8096.000000 	8096.000000 	... 	6418.000000 	3933.000000 	3933.000000 	3933.000000 	3933.000000 	3933.000000 	3933.000000 	3933.000000 	3933.000000 	8884.000000
mean 	7.927881 	41.394452 	51.217271 	7.767692 	4.977649 	7.790326 	675.004847 	103.028656 	5.512504 	4.939752 	... 	2.671086 	27.677526 	103.989554 	4.985652 	51.736819 	271.792289 	15.500149 	6.352372 	2.888615 	9.727559
std 	0.189872 	22.563245 	24.862498 	4.119155 	2.583922 	0.272739 	155.633409 	15.756448 	3.640776 	1.818564 	... 	3.085612 	18.623621 	16.556916 	1.910989 	26.186148 	328.464143 	16.905756 	9.647268 	4.817334 	24.099368
min 	6.670000 	9.630000 	8.630000 	1.160000 	0.010000 	4.580000 	55.200000 	30.970000 	1.100000 	1.050000 	... 	-7.700000 	-0.020000 	4.300000 	1.130000 	6.590000 	2.890000 	-0.100000 	-4.400000 	-6.200000 	0.010000
25% 	7.830000 	24.940000 	34.975000 	5.850000 	3.160000 	7.650000 	590.375000 	96.300000 	2.900000 	4.100000 	... 	1.000000 	13.800000 	97.650000 	4.100000 	29.800000 	105.100000 	7.800000 	3.300000 	1.100000 	0.730000
50% 	7.950000 	36.500000 	46.860000 	6.980000 	4.480000 	7.820000 	652.500000 	104.645000 	4.620000 	4.780000 	... 	1.990000 	22.700000 	106.000000 	4.800000 	51.100000 	179.500000 	12.010000 	4.900000 	2.100000 	2.030000
75% 	8.050000 	50.670000 	64.080000 	8.320000 	6.180000 	7.960000 	737.325000 	111.600000 	6.800000 	5.500000 	... 	3.320000 	35.200000 	113.000000 	5.600000 	65.100000 	329.200000 	18.700000 	7.390000 	3.500000 	7.540000
max 	9.360000 	452.060000 	272.500000 	40.350000 	21.750000 	8.920000 	6806.600000 	150.500000 	24.330000 	15.100000 	... 	104.500000 	157.800000 	154.800000 	38.100000 	206.300000 	7037.190000 	490.370000 	495.790000 	195.720000 	361.700000
~~~
{: .output}

> ## Reading Other Data
>
> Read the data in `UKCEHThamesInitiative_SamplingSiteLocations.csv`
> (which should be in the `supporting-documents` folder)
> into a variable called `locations`
> and display the column data types.
>
> > ## Solution
> > To read in a CSV, we use `pd.read_csv` and pass the filename `'supporting-documents/UKCEHThamesInitiative_SamplingSiteLocations.csv'` to it.
> > The summary statistics can be displayed with the `DataFrame.info()` method.
> > ~~~
> > locations = pd.read_csv('supporting-documents/UKCEHThamesInitiative_SamplingSiteLocations.csv')
> > locations.info()
> > ~~~
> >{: .language-python}
> {: .solution}
{: .challenge}

> ## Writing Data
>
> As well as the `read_csv` function for reading data from a file,
> Pandas provides a `to_csv` function to write dataframes to files.
> Applying what you've learned about reading from files,
> write one of your dataframes to a file called `processed.csv`.
> You can use `help` to get information on how to use `to_csv`.
> > ## Solution
> > In order to write the DataFrame `locations` to a file called `processed.csv`, execute the following command:
> > ~~~
> > locations.to_csv('processed.csv')
> > ~~~
> >{: .language-python}
> > For help on `to_csv`, you could execute, for example:
> > ~~~
> > help(locations.to_csv)
> > ~~~
> >{: .language-python}
> > Note that `help(to_csv)` throws an error! This is a subtlety and is due to the fact that `to_csv` is NOT a function in
> > and of itself and the actual call is `locations.to_csv`.
> {: .solution}
{: .challenge}
