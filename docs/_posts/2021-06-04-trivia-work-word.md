---
layout: post
title:  "AO3 Trivia: Works With Most Words Part I"
date:   2021-06-04
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



# Finding The Works With Most Words

We use .nlargest() method on the word_count column to find the 10 works with most words on AO3.


```python
# use .nlargest() method
works.loc[works['word_count'].nlargest(10).index]
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
      <th>4978554</th>
      <td>2016-08-28</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>5078036.0</td>
      <td>22+541478+15918+126089+63182+12+741433+230931+...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6212214</th>
      <td>2014-06-22</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>4796066.0</td>
      <td>23+14+15322+109011+108231+108232+186363+600534...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3502920</th>
      <td>2018-07-14</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>4332910.0</td>
      <td>1026+109503+12695+16+24754629+116+37259+11+796...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6500931</th>
      <td>2013-10-06</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>3817471.0</td>
      <td>11+21+16+1133664+48012+48013+648995+16999+1090...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1996502</th>
      <td>2019-12-18</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>3456587.0</td>
      <td>12+3693074+14030081+10482076+8658412+8658409+1...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3102917</th>
      <td>2018-12-18</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>3312781.0</td>
      <td>12+254648+13714235+19334348+557795+1275+282154...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2146591</th>
      <td>2019-10-29</td>
      <td>vi</td>
      <td>True</td>
      <td>False</td>
      <td>3163926.0</td>
      <td>5450+14+9</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6010465</th>
      <td>2014-12-06</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>3085821.0</td>
      <td>13+116+22+23+14+1001939+245368+586439+261582+7...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6723756</th>
      <td>2013-02-16</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>2853949.0</td>
      <td>13+14+951+40167+6563+6560+6559+950+1056+109629...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>691137</th>
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
# save the subset in local csv file 
works.loc[works['word_count'].nlargest(10).index].to_csv("trivia/top-words-all-time.csv",index=False)
```

We can see from above the work with most words on AO3 has a word count of 5,078,036. In the following post, we're going to find more informtion with the tag ids.

# Works With Most Words By Year

Let's do something a little more complex than above. Let's find the work with most words in each year.

The steps are as follows:

1. splitting the data by year using .groupby();

2. selecting the word_count column;

3. using .idxmax() method to find the index of the maximum value in said column;

4. using the .loc() and above index to select rows.


```python
# clear memory
gc.collect()
```




    289




```python
# make sure the data column is in datetime format
works['creation date'] = pd.to_datetime(works['creation date'])
```


```python
# find works with most words by year

works.loc[works.groupby(by=[works['creation date'].dt.year]).word_count.idxmax()]
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
      <th>7268202</th>
      <td>2008-11-06</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>128163.0</td>
      <td>22+183+2390+1048+966+16+1000+968+184+2395+2379...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7262756</th>
      <td>2009-11-14</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>756596.0</td>
      <td>23+19+13+114941+63594+125727+134988</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7200298</th>
      <td>2010-04-14</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>1005091.0</td>
      <td>2246+14+78550+8096+95285+8133+95354+8094+8130+...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7094887</th>
      <td>2011-06-06</td>
      <td>zh</td>
      <td>True</td>
      <td>True</td>
      <td>1490481.0</td>
      <td>13+23+14+1039+20020+24+22</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6955954</th>
      <td>2012-04-16</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>1310636.0</td>
      <td>13+23+14+136512+972932+4937593+70650+1833+2417...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6500931</th>
      <td>2013-10-06</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>3817471.0</td>
      <td>11+21+16+1133664+48012+48013+648995+16999+1090...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6212214</th>
      <td>2014-06-22</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>4796066.0</td>
      <td>23+14+15322+109011+108231+108232+186363+600534...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5580243</th>
      <td>2015-09-20</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>1779264.0</td>
      <td>21+16+10767+248734+8005+28451+1000+192+12</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4978554</th>
      <td>2016-08-28</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>5078036.0</td>
      <td>22+541478+15918+126089+63182+12+741433+230931+...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4168858</th>
      <td>2017-09-23</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>2588814.0</td>
      <td>299359+299357+299358+2927+1371926+21+14+12</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3502920</th>
      <td>2018-07-14</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>4332910.0</td>
      <td>1026+109503+12695+16+24754629+116+37259+11+796...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1996502</th>
      <td>2019-12-18</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>3456587.0</td>
      <td>12+3693074+14030081+10482076+8658412+8658409+1...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>691137</th>
      <td>2020-10-17</td>
      <td>en</td>
      <td>False</td>
      <td>False</td>
      <td>2598127.0</td>
      <td>11+13999+2927+6276+2246+17+61</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>306970</th>
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




```python
# save the subset in local csv file
works.loc[works.groupby(by=[works['creation date'].dt.year]).word_count.idxmax()].to_csv("trivia/top-words-by-year.csv",index=False)
```
