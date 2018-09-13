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
pd.options.display.float_format = '{:.1f}'.format
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
```
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




