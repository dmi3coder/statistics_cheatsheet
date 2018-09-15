# Statistics cheatsheet
**NOTES:**

This repository is Python-oriented, please fork and notify authors for any other language. Forks will be added in the bottom of this screen

Instead of available methods orientation, this cheatsheet orients on problems that needs to be solved

## Prepare data
### Setup script

You can use this setup script to cover most of the fast-to-hack problems 

```python
from __future__ import print_function

import math
import random

from IPython import display
from matplotlib import cm
from matplotlib import gridspec
from matplotlib import pyplot as plt
import numpy as np
import pandas as pd

pd.options.display.max_rows = 100
pd.set_option('display.expand_frame_repr', False)
pd.set_option('display.max_columns', None)
pd.set_option('display.max_rows', None)
pd.options.display.float_format = '{:.5f}'.format
```

### Import data

```python
pd.read_csv(filename) # From a CSV file
pd.read_table(filename) # From a delimited text file (like TSV)
pd.read_excel(filename) # From an Excel file
pd.read_sql(query, connection_object) # Reads from a SQL table/database
pd.read_json(json_string) # Reads from a JSON formatted string, URL or file.
pd.read_html(url) # Parses an html URL, string or file and extracts tables to a list of dataframes
pd.read_clipboard() # Takes the contents of your clipboard and passes it to read_table()
pd.DataFrame(dict) # From a dict, keys for columns names, values for data as lists
```

### Data used in examples
```python
df = pd.DataFrame()
df['some_column'] = [random.gauss(-4,4) for _ in range(10000)]
df['other_column'] = [random.gauss(1,4) for _ in range(10000)]
```

## Distributions

### Histograms

Graph thatshows frequency of each value

```python
# fastest way to show
df.some_column.hist() 
# Customizable way to show
plt.hist([df.some_column, df.other_column], # you can supply one value
			bins=30,
			label=['some column', 'other column'],
			color=['purple','g'])
plt.ylabel("you can set y or x label like so")
```
![Histogram example](https://github.com/dmitrychaban/statistics_cheatsheet/raw/master/images/dist_hist_1.png)

#### Variance

Variance is a summary statistic intended to describe the variability or spread of a distribution.

### Effect size

Summary statistic intended to describe size of an effect. Also known as [Cohen's d](https://stackoverflow.com/questions/21532471/how-to-calculate-cohens-d-in-python)

```python
def CohenEffectSize(group1, group2):
    diff = group1.mean() - group2.mean()
    var1 = group1.var()
    var2 = group2.var()
    n1, n2 = len(group1), len(group2)
    pooled_var = (n1 * var1 + n2 * var2) / (n1 + n2)
    d = diff / math.sqrt(pooled_var)
    return d

CohenEffectSize(df.some_column, df.other_column) # -1.250605635267294
```
### Probability mass function (PMF)

Representation of a distribution as a function that maps from values to probabilities.

```python
pmf = df['some_column'].value_counts().sort_index() / len(df['some_column'])
pmf.plot(kind='bar')
```

#### Bin values

If you'll try to execute code above, you'll get pmf of each element as 1. Good idea to bin this values


```python
df['some_column_pmf'] = pd.cut(df['some_column'], [-20,-10,-5,0,5,10,20])
pmf = df['some_column_pmf'].value_counts().sort_index() / len(df['some_column_pmf'])
pmf.plot(kind='bar')
```

![Pmf bin image](https://github.com/dmitrychaban/statistics_cheatsheet/raw/master/images/dist_pmf_bin1.png)

### Cumulative distribution function (CDF)

A function that maps from values to their cumulative probabilities.

```python
def EvalCdf(sample, x):
    count = 0.0
    for value in sample:
        if value <= x:
            count += 1
    prob = count / len(sample)
    return prob
    
EvalCdf(df['some_column'], 5.0)
```

### Compute CDF hist

```python
#fastest way
df['some_column'].hist(cumulative=True)
```

![Cdf image](https://github.com/dmitrychaban/statistics_cheatsheet/raw/master/images/dist_cdf_1.png)

