---
layout: post
title:  "Rating Tags in Works Part II"
date:   2021-05-28
categories: data_cleaning 
tags: Python Pandas 
---

In part II, we continue to prepare the data for visualization.

* Table of Contents
{:toc}

# Loading File

In part I, we created a csv file named rating.csv. We'll load the file here along with works-20210226.csv from AO3 official data dump.


```python
# Load python libraries
import pandas as pd
```


```python
# Load works file
works= pd.read_csv("/home/pi/Downloads/works-20210226.csv")
```


```python
# Load rating.csv from part I
rating = pd.read_csv("rating.csv")
```


```python
# preview file
rating
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
      <th>id</th>
      <th>type</th>
      <th>name</th>
      <th>canonical</th>
      <th>cached_count</th>
      <th>merger_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>9</td>
      <td>Rating</td>
      <td>Not Rated</td>
      <td>True</td>
      <td>825385</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10</td>
      <td>Rating</td>
      <td>General Audiences</td>
      <td>True</td>
      <td>2115153</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>11</td>
      <td>Rating</td>
      <td>Teen And Up Audiences</td>
      <td>True</td>
      <td>2272688</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>12</td>
      <td>Rating</td>
      <td>Mature</td>
      <td>True</td>
      <td>1151260</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>13</td>
      <td>Rating</td>
      <td>Explicit</td>
      <td>True</td>
      <td>1238331</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>12766726</td>
      <td>Rating</td>
      <td>Teen &amp; Up Audiences</td>
      <td>False</td>
      <td>333</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# preview file
works
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
<p>7269693 rows × 7 columns</p>
</div>



# Data Cleaning

In part I, we went through detailed steps of how to clean and prepare the works DataFrame for visualization. Here we'll skip the explanation.


```python
# Drop NA value in tags column
works = works.dropna(subset = ['tags'])
```


```python
# Drop unwanted columns 
works_subset = works[['creation date','tags']].copy(deep=True)
works_subset
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
      <th>tags</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2021-02-26</td>
      <td>10+414093+1001939+4577144+1499536+110+4682892+...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2021-02-26</td>
      <td>10+20350917+34816907+23666027+23269305+2326930...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2021-02-26</td>
      <td>10+10613413+9780526+3763877+3741104+7657229+30...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2021-02-26</td>
      <td>10+15322+54862755+20595867+32994286+663+471751...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2021-02-26</td>
      <td>11+721553+54604+1439500+3938423+53483274+54862...</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>7269688</th>
      <td>2008-09-13</td>
      <td>78+77+84+101+104+105+106+23+13+16+70+933</td>
    </tr>
    <tr>
      <th>7269689</th>
      <td>2008-09-13</td>
      <td>78+77+84+107+23+10+16+70+933+616</td>
    </tr>
    <tr>
      <th>7269690</th>
      <td>2008-09-13</td>
      <td>77+78+69+108+109+62+110+23+9+111+16+70+10128+4858</td>
    </tr>
    <tr>
      <th>7269691</th>
      <td>2008-09-13</td>
      <td>112+113+13+114+16+115+101+117+118+119+120+116+...</td>
    </tr>
    <tr>
      <th>7269692</th>
      <td>2008-09-13</td>
      <td>123+124+125+127+128+13+129+14+130+131+132+133+...</td>
    </tr>
  </tbody>
</table>
<p>7269690 rows × 2 columns</p>
</div>



# Clearing Memory

To release unreferenced memory, we use gc.collect().


```python
import gc
```


```python
del works
gc.collect()
```




    88



# Rating Column


```python
# Function to find the mimnimum value in the string, and return that value
def find_rating(x):
    return min([int(n) for n in x.split('+')])
```


```python
# Create a new column named 'rating'
# Use apply() to apply a function to each row
works_subset['rating'] = works_subset['tags'].apply(lambda x: find_rating(x))
```


```python
# Drop works with no rating
# Drop tags column
works_subset = works_subset[works_subset['rating'].isin(rating['id'])].drop('tags',axis=1)
```

# Preparation for Visualization


```python
# Clear Memory
gc.collect()
```




    88




```python
# Preview file
works_subset
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
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2021-02-26</td>
      <td>10</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2021-02-26</td>
      <td>10</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2021-02-26</td>
      <td>10</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2021-02-26</td>
      <td>10</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2021-02-26</td>
      <td>11</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>7269688</th>
      <td>2008-09-13</td>
      <td>13</td>
    </tr>
    <tr>
      <th>7269689</th>
      <td>2008-09-13</td>
      <td>10</td>
    </tr>
    <tr>
      <th>7269690</th>
      <td>2008-09-13</td>
      <td>9</td>
    </tr>
    <tr>
      <th>7269691</th>
      <td>2008-09-13</td>
      <td>13</td>
    </tr>
    <tr>
      <th>7269692</th>
      <td>2008-09-13</td>
      <td>13</td>
    </tr>
  </tbody>
</table>
<p>7269202 rows × 2 columns</p>
</div>




```python
# Make sure date column is in datetime format
works_subset['creation date'] = pd.to_datetime(works_subset['creation date'])
```


```python
# Group data by month and by rating count
# Group the date column by "month" (freq="1M")
# Pd.Grouper(key, freq) is used instead of pd.Series.dt.month
# because it does not aggregate month over multiple years
# Group the rating by counting each rating

subset_group = works_subset.groupby([pd.Grouper(key='creation date',freq='1M'),'rating']).size()
```


```python
# Reset index
subset_group = subset_group.reset_index()
```


```python
# Rename columns
subset_group.rename(columns={0:'count'}, inplace=True)
```


```python
subset_group
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
      <th>rating</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2008-09-30</td>
      <td>9</td>
      <td>76</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2008-09-30</td>
      <td>10</td>
      <td>232</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2008-09-30</td>
      <td>11</td>
      <td>213</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2008-09-30</td>
      <td>12</td>
      <td>174</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2008-09-30</td>
      <td>13</td>
      <td>233</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>745</th>
      <td>2021-02-28</td>
      <td>9</td>
      <td>15863</td>
    </tr>
    <tr>
      <th>746</th>
      <td>2021-02-28</td>
      <td>10</td>
      <td>42624</td>
    </tr>
    <tr>
      <th>747</th>
      <td>2021-02-28</td>
      <td>11</td>
      <td>46716</td>
    </tr>
    <tr>
      <th>748</th>
      <td>2021-02-28</td>
      <td>12</td>
      <td>23610</td>
    </tr>
    <tr>
      <th>749</th>
      <td>2021-02-28</td>
      <td>13</td>
      <td>25034</td>
    </tr>
  </tbody>
</table>
<p>750 rows × 3 columns</p>
</div>




```python
# Edit data format into pivot table
subset_pivot = subset_group.pivot(index = 'creation date', columns = 'rating', values='count')
subset_pivot
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
      <th>rating</th>
      <th>9</th>
      <th>10</th>
      <th>11</th>
      <th>12</th>
      <th>13</th>
    </tr>
    <tr>
      <th>creation date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2008-09-30</th>
      <td>76</td>
      <td>232</td>
      <td>213</td>
      <td>174</td>
      <td>233</td>
    </tr>
    <tr>
      <th>2008-10-31</th>
      <td>38</td>
      <td>111</td>
      <td>93</td>
      <td>43</td>
      <td>196</td>
    </tr>
    <tr>
      <th>2008-11-30</th>
      <td>11</td>
      <td>97</td>
      <td>97</td>
      <td>56</td>
      <td>76</td>
    </tr>
    <tr>
      <th>2008-12-31</th>
      <td>2</td>
      <td>93</td>
      <td>47</td>
      <td>41</td>
      <td>56</td>
    </tr>
    <tr>
      <th>2009-01-31</th>
      <td>18</td>
      <td>175</td>
      <td>104</td>
      <td>78</td>
      <td>133</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2020-10-31</th>
      <td>14188</td>
      <td>42416</td>
      <td>47706</td>
      <td>22015</td>
      <td>28723</td>
    </tr>
    <tr>
      <th>2020-11-30</th>
      <td>13397</td>
      <td>38003</td>
      <td>42168</td>
      <td>19005</td>
      <td>21743</td>
    </tr>
    <tr>
      <th>2020-12-31</th>
      <td>15763</td>
      <td>50443</td>
      <td>51435</td>
      <td>22664</td>
      <td>26656</td>
    </tr>
    <tr>
      <th>2021-01-31</th>
      <td>16875</td>
      <td>45592</td>
      <td>51099</td>
      <td>23830</td>
      <td>27084</td>
    </tr>
    <tr>
      <th>2021-02-28</th>
      <td>15863</td>
      <td>42624</td>
      <td>46716</td>
      <td>23610</td>
      <td>25034</td>
    </tr>
  </tbody>
</table>
<p>150 rows × 5 columns</p>
</div>



# Exporting To CSV File

In order to quickly access the DataFrame in part III, we export it to a local csv file.


```python
subset_pivot.to_csv("rating_pivot.csv")
```
