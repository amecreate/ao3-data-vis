---
layout: post
title:  "Tags on AO3: Visualization with Pie Chart"
date:   2021-05-23
category: visualization
tags: Python Pandas Matplotlib pie-chart table
---

![png]({{ "assets/0523piechart/output_26_0.png" | relative_url }})

In this section, we're going to work on the other file from the AO3 data dump, the tags. 

* Table of Contents
{:toc}

# Loading File


```python
# Load Python library
import pandas as pd

# Load file
path="/home/pi/Downloads/tags-20210226.csv"
chunker = pd.read_csv(path, chunksize=10000)
tags = pd.concat(chunker, ignore_index=True)
```


```python
# Preview file
tags
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
      <td>1</td>
      <td>Media</td>
      <td>TV Shows</td>
      <td>True</td>
      <td>910</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Media</td>
      <td>Movies</td>
      <td>True</td>
      <td>1164</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Media</td>
      <td>Books &amp; Literature</td>
      <td>True</td>
      <td>134</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Media</td>
      <td>Cartoons &amp; Comics &amp; Graphic Novels</td>
      <td>True</td>
      <td>166</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Media</td>
      <td>Anime &amp; Manga</td>
      <td>True</td>
      <td>501</td>
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
    </tr>
    <tr>
      <th>14467133</th>
      <td>55395603</td>
      <td>Freeform</td>
      <td>Redacted</td>
      <td>False</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14467134</th>
      <td>55395606</td>
      <td>Freeform</td>
      <td>Redacted</td>
      <td>False</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14467135</th>
      <td>55395609</td>
      <td>Freeform</td>
      <td>Redacted</td>
      <td>False</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14467136</th>
      <td>55395612</td>
      <td>Freeform</td>
      <td>Redacted</td>
      <td>False</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14467137</th>
      <td>55395615</td>
      <td>Freeform</td>
      <td>Redacted</td>
      <td>False</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>14467138 rows × 6 columns</p>
</div>



# Exploring Data


```python
# Find tag types
tags.type.unique()
```




    array(['Media', 'Rating', 'ArchiveWarning', 'Category', 'Character',
           'Fandom', 'Relationship', 'Freeform', 'UnsortedTag'], dtype=object)




```python
# Find subcategories of Media type tag
tags[tags['type'] == 'Media'].name.unique()
```




    array(['TV Shows', 'Movies', 'Books & Literature',
           'Cartoons & Comics & Graphic Novels', 'Anime & Manga',
           'Music & Bands', 'Celebrities & Real People', 'Other Media',
           'Video Games', 'No Media', 'Uncategorized Fandoms', 'Theater'],
          dtype=object)



# Cleaning and Manipulating Data

Visualize the Media tags according to its cached_count


```python
# Visualize the Media tags according to its cached_count
# Prepare data set
# Select columns

subset = tags[['type', 'name', 'cached_count']].copy()
subset
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
      <th>type</th>
      <th>name</th>
      <th>cached_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Media</td>
      <td>TV Shows</td>
      <td>910</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Media</td>
      <td>Movies</td>
      <td>1164</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Media</td>
      <td>Books &amp; Literature</td>
      <td>134</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Media</td>
      <td>Cartoons &amp; Comics &amp; Graphic Novels</td>
      <td>166</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Media</td>
      <td>Anime &amp; Manga</td>
      <td>501</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>14467133</th>
      <td>Freeform</td>
      <td>Redacted</td>
      <td>0</td>
    </tr>
    <tr>
      <th>14467134</th>
      <td>Freeform</td>
      <td>Redacted</td>
      <td>0</td>
    </tr>
    <tr>
      <th>14467135</th>
      <td>Freeform</td>
      <td>Redacted</td>
      <td>0</td>
    </tr>
    <tr>
      <th>14467136</th>
      <td>Freeform</td>
      <td>Redacted</td>
      <td>0</td>
    </tr>
    <tr>
      <th>14467137</th>
      <td>Freeform</td>
      <td>Redacted</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>14467138 rows × 3 columns</p>
</div>




```python
# Select all names that type is media

media = subset[subset['type'] == 'Media'].drop('type',axis=1)
media
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
      <th>name</th>
      <th>cached_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>TV Shows</td>
      <td>910</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Movies</td>
      <td>1164</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Books &amp; Literature</td>
      <td>134</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Cartoons &amp; Comics &amp; Graphic Novels</td>
      <td>166</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Anime &amp; Manga</td>
      <td>501</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Music &amp; Bands</td>
      <td>19</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Celebrities &amp; Real People</td>
      <td>33</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Other Media</td>
      <td>11</td>
    </tr>
    <tr>
      <th>388</th>
      <td>Video Games</td>
      <td>448</td>
    </tr>
    <tr>
      <th>4510</th>
      <td>No Media</td>
      <td>0</td>
    </tr>
    <tr>
      <th>8150</th>
      <td>Uncategorized Fandoms</td>
      <td>1</td>
    </tr>
    <tr>
      <th>23433</th>
      <td>Theater</td>
      <td>104</td>
    </tr>
  </tbody>
</table>
</div>



# Matplotlib Pie Chart


```python
# Import libraries
# Top line is Jupyter Notebook specific

%matplotlib inline

import matplotlib.pyplot as plt
```


```python
# Visualization pie chart
# with matplotlib

explode = [0.1] * len(media.name)

fig, axes = plt.subplots()
ax = axes.pie(media.cached_count, labels=media.name, explode=explode, autopct='%1.1f%%')
```


    
![png]({{ "assets/0523piechart/output_13_0.png" | relative_url }})
    


# Modifying data set for a better graph

Sort the data, find quantile, group lower values together


```python
# Sort the dataframe in descending order
# Use inplace=True to edit the existing dataframe

media.sort_values(by='cached_count',ascending=False, inplace=True, ignore_index=True)
media
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
      <th>name</th>
      <th>cached_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Movies</td>
      <td>1164</td>
    </tr>
    <tr>
      <th>1</th>
      <td>TV Shows</td>
      <td>910</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Anime &amp; Manga</td>
      <td>501</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Video Games</td>
      <td>448</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Cartoons &amp; Comics &amp; Graphic Novels</td>
      <td>166</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Books &amp; Literature</td>
      <td>134</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Theater</td>
      <td>104</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Celebrities &amp; Real People</td>
      <td>33</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Music &amp; Bands</td>
      <td>19</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Other Media</td>
      <td>11</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Uncategorized Fandoms</td>
      <td>1</td>
    </tr>
    <tr>
      <th>11</th>
      <td>No Media</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Find 50th quantile values

qt_50 = media.cached_count.quantile(.50)
qt_50
```




    119.0




```python
# Group values below 50th quantile
# Find what rows to drop
drop_rows = media[media['cached_count'] <= qt_50].copy()
drop_rows
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
      <th>name</th>
      <th>cached_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6</th>
      <td>Theater</td>
      <td>104</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Celebrities &amp; Real People</td>
      <td>33</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Music &amp; Bands</td>
      <td>19</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Other Media</td>
      <td>11</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Uncategorized Fandoms</td>
      <td>1</td>
    </tr>
    <tr>
      <th>11</th>
      <td>No Media</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Find the sum of drop_rows values

add_value = media[media['cached_count'] <= qt_50].cached_count.sum()
add_value
```




    168




```python
# Edit media dataframe
# Add a new row Other
# Add new value as cached_count

media2 = media[media['cached_count'] > qt_50].append({'name':'Other','cached_count':add_value}, ignore_index=True)
media2
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
      <th>name</th>
      <th>cached_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Movies</td>
      <td>1164</td>
    </tr>
    <tr>
      <th>1</th>
      <td>TV Shows</td>
      <td>910</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Anime &amp; Manga</td>
      <td>501</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Video Games</td>
      <td>448</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Cartoons &amp; Comics &amp; Graphic Novels</td>
      <td>166</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Books &amp; Literature</td>
      <td>134</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Other</td>
      <td>168</td>
    </tr>
  </tbody>
</table>
</div>




```python
# pie chart with new media2 dataframe

explode = [0.1] * len(media2.name)

fig,axes = plt.subplots()

ax = axes.pie(media2.cached_count, labels=media2.name, explode=explode, autopct='%1.1f%%')

```


    
![png]({{ "assets/0523piechart/output_20_0.png" | relative_url }})
    


# Matplotlib Table


```python
# Test pandas.plotting.table()

fig,ax = plt.subplots()
ax = pd.plotting.table(ax,data=drop_rows)
```


    
![png]({{ "assets/0523piechart/output_22_0.png" | relative_url }})
    



```python
# Edit drop_rows dataframe
# Add a new column of percentage
drop_rows['Percentage'] = (drop_rows.cached_count / media.cached_count.sum() * 100).round(2)
drop_rows
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
      <th>name</th>
      <th>cached_count</th>
      <th>Percentage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6</th>
      <td>Theater</td>
      <td>104</td>
      <td>2.98</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Celebrities &amp; Real People</td>
      <td>33</td>
      <td>0.95</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Music &amp; Bands</td>
      <td>19</td>
      <td>0.54</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Other Media</td>
      <td>11</td>
      <td>0.32</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Uncategorized Fandoms</td>
      <td>1</td>
      <td>0.03</td>
    </tr>
    <tr>
      <th>11</th>
      <td>No Media</td>
      <td>0</td>
      <td>0.00</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Drop cached_count
# Change column name
drop_rows.drop('cached_count', axis=1, inplace=True)
drop_rows.columns = ['Other', 'Percentage']
drop_rows
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
      <th>Other</th>
      <th>Percentage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6</th>
      <td>Theater</td>
      <td>2.98</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Celebrities &amp; Real People</td>
      <td>0.95</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Music &amp; Bands</td>
      <td>0.54</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Other Media</td>
      <td>0.32</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Uncategorized Fandoms</td>
      <td>0.03</td>
    </tr>
    <tr>
      <th>11</th>
      <td>No Media</td>
      <td>0.00</td>
    </tr>
  </tbody>
</table>
</div>



# Plotting Pie Chart and Table


```python
# Plot pie chart
# Plot table 
explode = [0.1] * len(media2.name)

fig,axes = plt.subplots(figsize=(10, 8))

ax = axes.pie(media2.cached_count, labels=media2.name, explode=explode, autopct='%1.1f%%')
table = pd.plotting.table(axes,data=drop_rows, cellLoc='center')

# Aaethetics
table.scale(1,1.5) # add padding around cell text
ax = plt.title('AO3 Tags Breakdown By Media Type \n 2008-2021')
```


    
![png]({{ "assets/0523piechart/output_26_0.png" | relative_url }})
    

