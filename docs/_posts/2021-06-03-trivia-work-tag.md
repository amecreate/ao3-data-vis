---
layout: post
title:  "AO3 Trivia: Works With Most Tags"
date:   2021-06-03
categories: data-exploration
tags: Python Pandas
---

In this series of posts, we look at some fun facts on AO3, such as:

- works with most tags;

- works with most words;

- top fandoms;

etc.

* Table of Contents
{:toc}

# Loading File


```python
# Load python libraries
import pandas as pd
import gc
```


```python
# Load data

chunker = pd.read_csv("/home/pi/Downloads/works-20210226.csv", chunksize=10000)
works = pd.concat(chunker, ignore_index=True)
```


```python
# Preview
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



# Finding The Number Of Tags

Each row in the tags column is a string with the plus sign separating tag ids. To find the number of tags, we simply split the string into a list by the plus sign, and find the length of the list.


```python
# Drop na value
works = works.dropna(subset=["tags"])
```


```python
# Add a tag length column
works["tag_len"] = works["tags"].apply(lambda x: len(x.split("+")))
```


```python
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
      <th>tag_len</th>
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
      <td>9</td>
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
      <td>17</td>
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
      <td>21</td>
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
      <td>14</td>
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
      <td>17</td>
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
      <td>12</td>
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
      <td>10</td>
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
      <td>14</td>
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
      <td>14</td>
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
      <td>21</td>
    </tr>
  </tbody>
</table>
<p>7269690 rows × 8 columns</p>
</div>



# nlargest() Method

The [pandas.DataFrame.nlargest](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.nlargest.html) returns the first n rows with the largest values in columns, in descending order.

We use the method to find the top 10 works with most tags.


```python
# Find the index of the top 10 works
works.tag_len.nlargest(10).index
```




    Int64Index([3892613, 3587796, 1883307, 4207234,  902616, 4203009, 1671832,
                1223902, 2128078, 3039343],
               dtype='int64')




```python
# Find the actual subset of dataframe with above index
# Note .loc returns a label of the index. 
# This use is not an integer position along the index.

works.loc[works.tag_len.nlargest(10).index]
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
      <th>tag_len</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3892613</th>
      <td>2018-01-27</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>9584.0</td>
      <td>12+58950+10861138+8191561+21029181+21029184+49...</td>
      <td>NaN</td>
      <td>678</td>
    </tr>
    <tr>
      <th>3587796</th>
      <td>2018-06-10</td>
      <td>en</td>
      <td>True</td>
      <td>False</td>
      <td>9086.0</td>
      <td>12+1909262+23913063+3247121+15425106+5149081+9...</td>
      <td>NaN</td>
      <td>629</td>
    </tr>
    <tr>
      <th>1883307</th>
      <td>2020-01-17</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>1825.0</td>
      <td>6541412+6874891+6679306+11004304+10959982+6799...</td>
      <td>NaN</td>
      <td>599</td>
    </tr>
    <tr>
      <th>4207234</th>
      <td>2017-09-04</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>77028.0</td>
      <td>13+576450+4182+245530+143056+1487281+1050015+1...</td>
      <td>NaN</td>
      <td>548</td>
    </tr>
    <tr>
      <th>902616</th>
      <td>2020-09-01</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>5100.0</td>
      <td>12+1002903+6841411+2467008+1775227+5556795+116...</td>
      <td>NaN</td>
      <td>518</td>
    </tr>
    <tr>
      <th>4203009</th>
      <td>2017-09-06</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>124766.0</td>
      <td>2692+18335742+18335451+18335454+18335457+18335...</td>
      <td>NaN</td>
      <td>517</td>
    </tr>
    <tr>
      <th>1671832</th>
      <td>2020-03-14</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>64835.0</td>
      <td>13+448284+9607162+38719528+38712538+292740+746...</td>
      <td>NaN</td>
      <td>497</td>
    </tr>
    <tr>
      <th>1223902</th>
      <td>2020-06-27</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>31814.0</td>
      <td>2692+14+12+2927+479641+494522+844577+475944+47...</td>
      <td>NaN</td>
      <td>462</td>
    </tr>
    <tr>
      <th>2128078</th>
      <td>2019-11-04</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>21716.0</td>
      <td>11+758208+1786102+770175+23+14+3393344+4696920...</td>
      <td>NaN</td>
      <td>441</td>
    </tr>
    <tr>
      <th>3039343</th>
      <td>2019-01-05</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>66413.0</td>
      <td>10+26034884+565209+27204215+27175544+60+2472+1...</td>
      <td>NaN</td>
      <td>435</td>
    </tr>
  </tbody>
</table>
</div>



The above subset displays the top 10 works with most tags. The first work has a total of 678 tags. 

In the following posts, we're going to replace tag ids with tag names in order to find more information about the works.
