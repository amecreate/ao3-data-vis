---
layout: post
title:  "Columns, N/A Values, and Simple Data Cleaning"
date:   2021-05-07
category: data_cleaning
tags: Python Pandas 
---

In this section, we focus on navigating the data set, and cleaning missing values. 

* Table of Contents
{:toc}

# Loading File

For details on how to load large csv files in Python, check out [Loading CSV Files in Python]({{ 2021-05-06-load-file.md | relative_url }}). 


```python
# Load Python library
import pandas as pd

# Load file
path="/home/pi/Downloads/works-20210226.csv"
chunker = pd.read_csv(path, chunksize=10000)
works = pd.concat(chunker, ignore_index=True)
```

# Dataframe Shape

We can use ```shape``` to get the number of rows and columns in the data set. Specifically, shape[0] displays the number of rows, and shape[1] displays the number of columns.


```python
# The number of rows and columns in the dataframe
works.shape
```




    (7269693, 7)




```python
# Number of rows
works.shape[0]
```




    7269693




```python
# Number of cols
works.shape[1]
```




    7



As we can see from above, there are 7269693 rows and 7 columns in the dataframe.

# Column Names and Selecting Columns

To preview the data set and its columns, we can print out the first few rows. However, sometimes there are too many columns in a data set it is difficult to display on the screen. Instead, we could print the column names separately.


```python
# View col names
works.columns
```




    Index(['creation date', 'language', 'restricted', 'complete', 'word_count',
           'tags', 'Unnamed: 6'],
          dtype='object')



Often, we only need certain columns to work with. There are several ways to select columns, let's select language column for example.


```python
# Select a single column 
works.language
```




    0          en
    1          en
    2          en
    3          en
    4          en
               ..
    7269688    en
    7269689    en
    7269690    en
    7269691    en
    7269692    en
    Name: language, Length: 7269693, dtype: object




```python
works['language']
```




    0          en
    1          en
    2          en
    3          en
    4          en
               ..
    7269688    en
    7269689    en
    7269690    en
    7269691    en
    7269692    en
    Name: language, Length: 7269693, dtype: object




```python
works.loc[:,'language']
```




    0          en
    1          en
    2          en
    3          en
    4          en
               ..
    7269688    en
    7269689    en
    7269690    en
    7269691    en
    7269692    en
    Name: language, Length: 7269693, dtype: object



As we can see from above, the three methods yield exactly the same results. Which one to use depends on your own preference.

Let's select multiple columns at the same time.


```python
# Select multiple columns
works[['creation date','language']]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>creation date</th>
      <th>language</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2021-02-26</td>
      <td>en</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2021-02-26</td>
      <td>en</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2021-02-26</td>
      <td>en</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2021-02-26</td>
      <td>en</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2021-02-26</td>
      <td>en</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>7269688</th>
      <td>2008-09-13</td>
      <td>en</td>
    </tr>
    <tr>
      <th>7269689</th>
      <td>2008-09-13</td>
      <td>en</td>
    </tr>
    <tr>
      <th>7269690</th>
      <td>2008-09-13</td>
      <td>en</td>
    </tr>
    <tr>
      <th>7269691</th>
      <td>2008-09-13</td>
      <td>en</td>
    </tr>
    <tr>
      <th>7269692</th>
      <td>2008-09-13</td>
      <td>en</td>
    </tr>
  </tbody>
</table>
<p>7269693 rows × 2 columns</p>
</div>




```python
works.loc[:,['creation date', 'language']]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>creation date</th>
      <th>language</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2021-02-26</td>
      <td>en</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2021-02-26</td>
      <td>en</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2021-02-26</td>
      <td>en</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2021-02-26</td>
      <td>en</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2021-02-26</td>
      <td>en</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>7269688</th>
      <td>2008-09-13</td>
      <td>en</td>
    </tr>
    <tr>
      <th>7269689</th>
      <td>2008-09-13</td>
      <td>en</td>
    </tr>
    <tr>
      <th>7269690</th>
      <td>2008-09-13</td>
      <td>en</td>
    </tr>
    <tr>
      <th>7269691</th>
      <td>2008-09-13</td>
      <td>en</td>
    </tr>
    <tr>
      <th>7269692</th>
      <td>2008-09-13</td>
      <td>en</td>
    </tr>
  </tbody>
</table>
<p>7269693 rows × 2 columns</p>
</div>



With the two columns on hand, we can proceed with more in-depth analysis. For example, we could:

- Find the total number of languages on AO3
- Find top 5 most popular languages on AO3
- Visulize language trend
- Analyze users' posting habits

etc.

But before we can do any analysis, we need to clean the data set.

# N/A values

In real world, the data set may contain missing values (showing up as NaN). In order to prepare the data for further analysis, we need to detect null values, single out the rows, and eventually drop the rows containing null values.

For more details behind the scene, check out [Handling Missing Data](https://jakevdp.github.io/PythonDataScienceHandbook/03.04-missing-values.html).


```python
# Detect null values in data
works.isna().any()
```




    creation date    False
    language          True
    restricted       False
    complete         False
    word_count        True
    tags              True
    Unnamed: 6        True
    dtype: bool




```python
# Alternatively using isnull(), same results
works.isnull().any()
```




    creation date    False
    language          True
    restricted       False
    complete         False
    word_count        True
    tags              True
    Unnamed: 6        True
    dtype: bool



Here it shows that the language, word_count and tages columns all have null values. Let's check out what they look like before taking any actions regarding the null values.


```python
# Select language column, display only rows containing null values
works['language'][works['language'].isnull()]
```




    73119      NaN
    95222      NaN
    184633     NaN
    211955     NaN
    266702     NaN
              ... 
    6197226    NaN
    6210472    NaN
    6216530    NaN
    6266535    NaN
    6272792    NaN
    Name: language, Length: 90, dtype: object




```python
# Same method, word_count column
works['word_count'][works['word_count'].isnull()]
```




    1404      NaN
    3846      NaN
    3976      NaN
    5458      NaN
    6170      NaN
               ..
    6531822   NaN
    6559452   NaN
    6755516   NaN
    6847505   NaN
    6897542   NaN
    Name: word_count, Length: 2268, dtype: float64



If we decide to drop all rows that has at least one null value, we could:


```python
# Drop null values in language column
works.dropna(subset = ['language'])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>creation date</th>
      <th>language</th>
      <th>restricted</th>
      <th>complete</th>
      <th>word_count</th>
      <th>tags</th>
      <th>Unnamed: 6</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2021-02-26</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>388.0</td>
      <td>10+414093+1001939+4577144+1499536+110+4682892+...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2021-02-26</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>1638.0</td>
      <td>10+20350917+34816907+23666027+23269305+2326930...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2021-02-26</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>1502.0</td>
      <td>10+10613413+9780526+3763877+3741104+7657229+30...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2021-02-26</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>100.0</td>
      <td>10+15322+54862755+20595867+32994286+663+471751...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2021-02-26</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>994.0</td>
      <td>11+721553+54604+1439500+3938423+53483274+54862...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>7269688</th>
      <td>2008-09-13</td>
      <td>en</td>
      <td>True</td>
      <td>True</td>
      <td>705.0</td>
      <td>78+77+84+101+104+105+106+23+13+16+70+933</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7269689</th>
      <td>2008-09-13</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>1392.0</td>
      <td>78+77+84+107+23+10+16+70+933+616</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7269690</th>
      <td>2008-09-13</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>1755.0</td>
      <td>77+78+69+108+109+62+110+23+9+111+16+70+10128+4858</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7269691</th>
      <td>2008-09-13</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>1338.0</td>
      <td>112+113+13+114+16+115+101+117+118+119+120+116+...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7269692</th>
      <td>2008-09-13</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>1836.0</td>
      <td>123+124+125+127+128+13+129+14+130+131+132+133+...</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>7269603 rows × 7 columns</p>
</div>



Compared to the 7269693 rows in original data set, we dropped 90 rows that contain missing values in the language column, which is exactly what we intended to achieve.
