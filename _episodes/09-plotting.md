---
title: "Plotting"
teaching: 15
exercises: 15
questions:
- "How can I plot my data?"
- "How can I save my plot for publishing?"
objectives:
- "Create a time series plot showing a single data set."
- "Create a scatter plot showing relationship between two data sets."
keypoints:
- "[`matplotlib`](https://matplotlib.org/) is the most widely used scientific plotting library in Python."
- "Plot data directly from a Pandas dataframe."
- "Select and transform data, then plot it."
- "Many styles of plot are available: see the [Python Graph Gallery](https://python-graph-gallery.com/matplotlib/) for more options."
- "Can plot many sets of data together."
---
## [`matplotlib`](https://matplotlib.org/) is the most widely used scientific plotting library in Python.

*   Commonly use a sub-library called [`matplotlib.pyplot`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.html#module-matplotlib.pyplot).
*   The Jupyter Notebook will render plots inline by default.

~~~
import matplotlib.pyplot as plt
~~~
{: .language-python}

*   Simple plots are then (fairly) simple to create.

~~~
time = [0, 1, 2, 3]
position = [0, 100, 200, 300]

fig, ax = plt.subplots()
ax.plot(time, position)
ax.xlabel('Time (hr)')
ax.ylabel('Position (km)')
~~~
{: .language-python}

![Simple Position-Time Plot](../fig/9_simple_position_time_plot.svg)

> ## Display All Open Figures
>
> In our Jupyter Notebook example, running the cell should generate the figure directly below the code.
> The figure is also included in the Notebook document for future viewing.
> However, other Python environments like an interactive Python session started from a terminal
> or a Python script executed via the command line require an additional command to display the figure.
>
> Instruct `matplotlib` to show a figure:
> ~~~
> plt.show()
> ~~~
> {: .language-python}
>
> This command can also be used within a Notebook - for instance, to display multiple figures
> if several are created by a single cell.
>
{: .callout}

## Plot data directly from a [`Pandas dataframe`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html).

*   We can also plot [Pandas dataframes](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html).
*   This implicitly uses [`matplotlib.pyplot`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.html#module-matplotlib.pyplot).
*   First, process the data to create a datetime index

~~~
import pandas as pd
import matplotlib.pyplot as plt

data = pd.read_csv("data/Thames_Initiative_2009-2017.csv")
data["datetime"] = pd.to_datetime(data["Sampling Date"] + " " + data["Time of sampling"])
data = data.set_index(data.datetime)

dissolved_cols = [col for col in data.columns if col.startswith("Dissolved")]

site_name = "Thames at Wallingford"

f, ax = plt.subplots(figsize=(12,6))
site = data[data["Site name"] == site_name]
ax = site.loc["2011", dissolved_cols].plot(ax=ax)

ax.set_title(site_name)
~~~
{: .language-python}

By default, [`DataFrame.plot`](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.plot.html#pandas.DataFrame.plot) plots with the rows as the X axis.

## Varying plot type

Back to using the matplotlib object oriented notation for fine control

We can vary the plot type...

~~~
maxima = data.groupby("Site name").max()
maxima["Total Ca (mg/l)"].plot(kind="bar")
~~~
{: .language-python}

One can over plot many datasets with repeated calls to plot functions like scatter, bar, hist, etc.

We can also add a legend by using the `label` keyword for each dataset and then calling ax.legend()

~~~
f, ax = plt.subplots(figsize=(12, 6))
alpha = 0.5
subset = data.iloc[::10, ]
ax.scatter(subset.index, subset.loc[:, "Total Cu (ug/l)"], c="red", label="Copper", alpha=alpha)
ax.scatter(subset.index, subset.loc[:, "Total Zn (ug/l)"], c="grey", label="Zinc", alpha=alpha)
ax.scatter(subset.index, subset.loc[:, "Total Mn (ug/l)"], c="purple", label="Manganese", alpha=alpha)
ax.set_ylim(0, 40)
ax.set_ylabel("Element concentration (ug/l)")
ax.set_xlabel("Time")
ax.legend()
~~~
{: .language-python}

Here's another plot type, a histogram

~~~
import numpy as np
f, ax = plt.subplots()
ax.hist(data["Mean daily flow (m3/s)"], bins=np.logspace(0, 2, 50))
ax.set_xscale("log")
ax.set_xlabel("Mean daily flow (m3/s)")
ax.set_ylabel("Frequency")
~~~
{: .language-python}


## Saving your plot to a file

~~~
f.savefig('plot.png')
~~~
{: .language-python}
