---
layout: post
title:  "AO3 and Its First Data Release"
date:   2021-05-05
category: visualization
tags: Python Pandas 
---

Archiveofourown.org (AO3) is a fan-created, fan-run, nonprofit, noncommercial archive for transformative fanworks. At the time of this writing, it has more than 42,750 fandoms, 3,547,000 users, and 7,428,000 works.

On 2021-03-21, the Archive released a [Selective data dump for fan statisticians](https://archiveofourown.org/admin_posts/18804). The data comes in two CSV files, described as follows:

The first includes information about works:

- creation date
- language
- word count
- restricted or not
- complete or not
- associated tag IDs

The second provides the key to the tag IDs:

- tag ID
- tag type (e.g. Warning, Fandom, Relationship)
- tag name (unless the tag has fewer than 5 uses)
- canonical or not
- an approximate number of uses
- merger ID (i.e. the tag's canonical version, if it has one)

There are endless possibilities with this data set, we can:

- Find out most popular languages
- Visualize language trend
- Analyze users' posting habits
- Look into the seasonality of users' writing habits
- Text mining and sentiment analysis 
- Word frequency, Topic modeling, etc

Let's take a sneak peek at what we have on hand.

The following code is written in Python and executed in Jupyter Notebook. You can follow along, or check out the [github repository](https://github.com/amecreate/ao3-data-vis) for this project. 


```python
# Load python libraries
import pandas as pd
# Sneak peek of the data that we're going to work on
pd.read_csv("/home/pi/Downloads/works-20210226.csv", nrows=5)
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
      <td>388</td>
      <td>10+414093+1001939+4577144+1499536+110+4682892+...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2021-02-26</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>1638</td>
      <td>10+20350917+34816907+23666027+23269305+2326930...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2021-02-26</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>1502</td>
      <td>10+10613413+9780526+3763877+3741104+7657229+30...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2021-02-26</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>100</td>
      <td>10+15322+54862755+20595867+32994286+663+471751...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2021-02-26</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>994</td>
      <td>11+721553+54604+1439500+3938423+53483274+54862...</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>


