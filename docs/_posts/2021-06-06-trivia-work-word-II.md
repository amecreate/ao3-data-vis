---
layout: post
title:  "AO3 Trivia: Works With Most Words Part II"
date:   2021-06-06
categories: data-exploration
tags: Python Pandas
---

In Part II, we look into the fandoms that have works with most words. 

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
chunker = pd.read_csv("/home/pi/Downloads/tags-20210226.csv", chunksize=10000)
tags = pd.concat(chunker, ignore_index=True)
```


```python
# preview
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



Note from the above preview, some tags have names as "Redacted". This is due to the fact that these tags have the cached_count less than 5. 

In Part I, we've extracted 10 works with most words on AO3, and saved it to a local file named "top-words-all-time.csv"; we also filtered the data by year, found the work with most words for that year, and saved the DataFrame as ""top-words-by-year.csv".


```python
# Load data
works_all = pd.read_csv("trivia/top-words-all-time.csv")
```


```python
works_all
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
      <td>2016-08-28</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>5078036.0</td>
      <td>22+541478+15918+126089+63182+12+741433+230931+...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2014-06-22</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>4796066.0</td>
      <td>23+14+15322+109011+108231+108232+186363+600534...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-07-14</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>4332910.0</td>
      <td>1026+109503+12695+16+24754629+116+37259+11+796...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2013-10-06</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>3817471.0</td>
      <td>11+21+16+1133664+48012+48013+648995+16999+1090...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2019-12-18</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>3456587.0</td>
      <td>12+3693074+14030081+10482076+8658412+8658409+1...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2018-12-18</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>3312781.0</td>
      <td>12+254648+13714235+19334348+557795+1275+282154...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2019-10-29</td>
      <td>vi</td>
      <td>True</td>
      <td>False</td>
      <td>3163926.0</td>
      <td>5450+14+9</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2014-12-06</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>3085821.0</td>
      <td>13+116+22+23+14+1001939+245368+586439+261582+7...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2013-02-16</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>2853949.0</td>
      <td>13+14+951+40167+6563+6560+6559+950+1056+109629...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2020-10-17</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>2598127.0</td>
      <td>11+13999+2927+6276+2246+17+61</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Load data
works_year = pd.read_csv("trivia/top-words-by-year.csv")
```


```python
works_year
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
      <td>2008-11-06</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>128163.0</td>
      <td>22+183+2390+1048+966+16+1000+968+184+2395+2379...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2009-11-14</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>756596.0</td>
      <td>23+19+13+114941+63594+125727+134988</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2010-04-14</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>1005091.0</td>
      <td>2246+14+78550+8096+95285+8133+95354+8094+8130+...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2011-06-06</td>
      <td>zh</td>
      <td>True</td>
      <td>True</td>
      <td>1490481.0</td>
      <td>13+23+14+1039+20020+24+22</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2012-04-16</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>1310636.0</td>
      <td>13+23+14+136512+972932+4937593+70650+1833+2417...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2013-10-06</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>3817471.0</td>
      <td>11+21+16+1133664+48012+48013+648995+16999+1090...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2014-06-22</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>4796066.0</td>
      <td>23+14+15322+109011+108231+108232+186363+600534...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2015-09-20</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>1779264.0</td>
      <td>21+16+10767+248734+8005+28451+1000+192+12</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2016-08-28</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>5078036.0</td>
      <td>22+541478+15918+126089+63182+12+741433+230931+...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2017-09-23</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>2588814.0</td>
      <td>299359+299357+299358+2927+1371926+21+14+12</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2018-07-14</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>4332910.0</td>
      <td>1026+109503+12695+16+24754629+116+37259+11+796...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2019-12-18</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>3456587.0</td>
      <td>12+3693074+14030081+10482076+8658412+8658409+1...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2020-10-17</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>2598127.0</td>
      <td>11+13999+2927+6276+2246+17+61</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2021-01-02</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>2261752.0</td>
      <td>839+30349+9830863+9830815+18525+262083+5195777...</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



# From Tag ID To Tag Name

On one hand, we have tag ids associated with works in one DataFrame; on the other hand, we have tag ids with names in another DataFrame. 

Let's split the tag id string into individual tag ids, find the respective name in tags DataFrame, and append the name back to the works DataFrame under a new column.

Because a work has multiple tags, some works have as many as 500+ tags (from previous post, Works With Most Tags), we'll only extract the fandom tag from the tags DataFrame. 

We can check the types of tags that are in the data set.


```python
# Types of tags
tags.type.unique()
```




    array(['Media', 'Rating', 'ArchiveWarning', 'Category', 'Character',
           'Fandom', 'Relationship', 'Freeform', 'UnsortedTag'], dtype=object)



In order to user .loc[] method to quickly select specific rows based on index labels, we first set tag id as index in the tags DataFrame.

## Set Index


```python
# Set id column as index
tags.set_index("id", inplace=True)
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
      <th>type</th>
      <th>name</th>
      <th>canonical</th>
      <th>cached_count</th>
      <th>merger_id</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Media</td>
      <td>TV Shows</td>
      <td>True</td>
      <td>910</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Media</td>
      <td>Movies</td>
      <td>True</td>
      <td>1164</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Media</td>
      <td>Books &amp; Literature</td>
      <td>True</td>
      <td>134</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Media</td>
      <td>Cartoons &amp; Comics &amp; Graphic Novels</td>
      <td>True</td>
      <td>166</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
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
    </tr>
    <tr>
      <th>55395603</th>
      <td>Freeform</td>
      <td>Redacted</td>
      <td>False</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>55395606</th>
      <td>Freeform</td>
      <td>Redacted</td>
      <td>False</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>55395609</th>
      <td>Freeform</td>
      <td>Redacted</td>
      <td>False</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>55395612</th>
      <td>Freeform</td>
      <td>Redacted</td>
      <td>False</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>55395615</th>
      <td>Freeform</td>
      <td>Redacted</td>
      <td>False</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>14467138 rows × 5 columns</p>
</div>



## Add_fandom function

We use a function to quickly split tags string into separate ids, use id to locate the tag name, and only select id names that are fandoms.


```python
# x is id string
# df is tags DataFrame
# The function returns all tag names that are Fandom type
# In case of multiple fandoms, tolist() is used 

def add_fandom(x,df):
    subset = df.loc[[int(n) for n in x.split("+")]]
    return subset[subset.type == "Fandom"].name.tolist()
```

## Apply to DataFrame


```python
# Add fandom to a new column
works_all["fandom"] = works_all["tags"].apply(lambda x: add_fandom(x,tags))
```


```python
works_all
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
      <th>fandom</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016-08-28</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>5078036.0</td>
      <td>22+541478+15918+126089+63182+12+741433+230931+...</td>
      <td>NaN</td>
      <td>[The Hobbit - All Media Types, The Lord of the...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2014-06-22</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>4796066.0</td>
      <td>23+14+15322+109011+108231+108232+186363+600534...</td>
      <td>NaN</td>
      <td>[Formula 1 RPF]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-07-14</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>4332910.0</td>
      <td>1026+109503+12695+16+24754629+116+37259+11+796...</td>
      <td>NaN</td>
      <td>[Terminator: The Sarah Connor Chronicles, Term...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2013-10-06</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>3817471.0</td>
      <td>11+21+16+1133664+48012+48013+648995+16999+1090...</td>
      <td>NaN</td>
      <td>[Fiction Wrestling - Fandom]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2019-12-18</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>3456587.0</td>
      <td>12+3693074+14030081+10482076+8658412+8658409+1...</td>
      <td>NaN</td>
      <td>[モブサイコ100 | Mob Psycho 100]</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2018-12-18</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>3312781.0</td>
      <td>12+254648+13714235+19334348+557795+1275+282154...</td>
      <td>NaN</td>
      <td>[Minecraft (Video Game)]</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2019-10-29</td>
      <td>vi</td>
      <td>True</td>
      <td>False</td>
      <td>3163926.0</td>
      <td>5450+14+9</td>
      <td>NaN</td>
      <td>[No Fandom]</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2014-12-06</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>3085821.0</td>
      <td>13+116+22+23+14+1001939+245368+586439+261582+7...</td>
      <td>NaN</td>
      <td>[The Avengers (Marvel Movies), Thor (Movies), ...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2013-02-16</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>2853949.0</td>
      <td>13+14+951+40167+6563+6560+6559+950+1056+109629...</td>
      <td>NaN</td>
      <td>[NCIS]</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2020-10-17</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>2598127.0</td>
      <td>11+13999+2927+6276+2246+17+61</td>
      <td>NaN</td>
      <td>[Naruto]</td>
    </tr>
  </tbody>
</table>
</div>



To summarize, the work with most words on AO3 is from The Hobbit fandom, followed by a work from Formula 1 RPF fandom. Interestingly, 9 works out of 10 are still works in progress. Note that the date in the data set is "creation date", meaning which year the work was created. They may well be updated regularly till this day. Thus, it is unsurprising that the works we see above are all created several years ago. It takes time to write fanfictions!


```python
# Same with works_year DataFrame
works_year["fandom"] = works_year["tags"].apply(lambda x: add_fandom(x,tags))
```


```python
works_year
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
      <th>fandom</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2008-11-06</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>128163.0</td>
      <td>22+183+2390+1048+966+16+1000+968+184+2395+2379...</td>
      <td>NaN</td>
      <td>[Harry Potter - Rowling]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2009-11-14</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>756596.0</td>
      <td>23+19+13+114941+63594+125727+134988</td>
      <td>NaN</td>
      <td>[Lord of the Rings - J. R. R. Tolkien]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2010-04-14</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>1005091.0</td>
      <td>2246+14+78550+8096+95285+8133+95354+8094+8130+...</td>
      <td>NaN</td>
      <td>[Weiß Kreuz]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2011-06-06</td>
      <td>zh</td>
      <td>True</td>
      <td>True</td>
      <td>1490481.0</td>
      <td>13+23+14+1039+20020+24+22</td>
      <td>NaN</td>
      <td>[Queer as Folk (US)]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2012-04-16</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>1310636.0</td>
      <td>13+23+14+136512+972932+4937593+70650+1833+2417...</td>
      <td>NaN</td>
      <td>[Harry Potter - J. K. Rowling]</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2013-10-06</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>3817471.0</td>
      <td>11+21+16+1133664+48012+48013+648995+16999+1090...</td>
      <td>NaN</td>
      <td>[Fiction Wrestling - Fandom]</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2014-06-22</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>4796066.0</td>
      <td>23+14+15322+109011+108231+108232+186363+600534...</td>
      <td>NaN</td>
      <td>[Formula 1 RPF]</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2015-09-20</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>1779264.0</td>
      <td>21+16+10767+248734+8005+28451+1000+192+12</td>
      <td>NaN</td>
      <td>[One Piece]</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2016-08-28</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>5078036.0</td>
      <td>22+541478+15918+126089+63182+12+741433+230931+...</td>
      <td>NaN</td>
      <td>[The Hobbit - All Media Types, The Lord of the...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2017-09-23</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>2588814.0</td>
      <td>299359+299357+299358+2927+1371926+21+14+12</td>
      <td>NaN</td>
      <td>[Wiedźmin | The Witcher (Video Game), Wiedźmin...</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2018-07-14</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>4332910.0</td>
      <td>1026+109503+12695+16+24754629+116+37259+11+796...</td>
      <td>NaN</td>
      <td>[Terminator: The Sarah Connor Chronicles, Term...</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2019-12-18</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>3456587.0</td>
      <td>12+3693074+14030081+10482076+8658412+8658409+1...</td>
      <td>NaN</td>
      <td>[モブサイコ100 | Mob Psycho 100]</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2020-10-17</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>2598127.0</td>
      <td>11+13999+2927+6276+2246+17+61</td>
      <td>NaN</td>
      <td>[Naruto]</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2021-01-02</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>2261752.0</td>
      <td>839+30349+9830863+9830815+18525+262083+5195777...</td>
      <td>NaN</td>
      <td>[X-Men (Movieverse), X-Men (Comicverse), X-Men...</td>
    </tr>
  </tbody>
</table>
</div>



Here, we have the works with most words created each year. Some works are also in the previous DataFram. 
