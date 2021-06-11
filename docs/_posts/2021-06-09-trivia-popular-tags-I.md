---
layout: post
title:  "AO3 Trivia: Popular Tags Part I"
date:   2021-06-09
categories: data-exploration visualization
tags: Python Pandas WordCloud 
---

![png]({{ "assets/output_23_1.png" | relative_url }})

In this section, we use the python [WordCloud](https://amueller.github.io/word_cloud/) library to visualize the popular tags on AO3.

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



# Tag Types

Here are the types of tags on AO3, we can browse and find popular tags under each type.


```python
tags.type.unique()
```




    array(['Media', 'Rating', 'ArchiveWarning', 'Category', 'Character',
           'Fandom', 'Relationship', 'Freeform', 'UnsortedTag'], dtype=object)



We've analyzed the media tags in a previous post "Tags on AO3: Visualization with Pie Chart", and the rating tags with bar-chart-race library in "Rating Tags in Works Part III". 

## Popular Characters


```python
# Top 10 most popular characters based on cached_count
tags.loc[tags[tags.type == "Character"].cached_count.nlargest(10).index]
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
      <th>24</th>
      <td>26</td>
      <td>Character</td>
      <td>Dean Winchester</td>
      <td>True</td>
      <td>196184</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2663</th>
      <td>3175</td>
      <td>Character</td>
      <td>Tony Stark</td>
      <td>True</td>
      <td>188737</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5975</th>
      <td>7267</td>
      <td>Character</td>
      <td>Steve Rogers</td>
      <td>True</td>
      <td>183041</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1535</th>
      <td>1803</td>
      <td>Character</td>
      <td>Harry Potter</td>
      <td>True</td>
      <td>180395</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2458</th>
      <td>2927</td>
      <td>Character</td>
      <td>Original Characters</td>
      <td>True</td>
      <td>170848</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>23</th>
      <td>25</td>
      <td>Character</td>
      <td>Sam Winchester</td>
      <td>True</td>
      <td>155832</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>277937</th>
      <td>479641</td>
      <td>Character</td>
      <td>Original Female Character(s)</td>
      <td>True</td>
      <td>155668</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>39135</th>
      <td>54604</td>
      <td>Character</td>
      <td>Reader</td>
      <td>True</td>
      <td>138216</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3796</th>
      <td>4622</td>
      <td>Character</td>
      <td>Sherlock Holmes</td>
      <td>True</td>
      <td>120949</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>56907</th>
      <td>80648</td>
      <td>Character</td>
      <td>James "Bucky" Barnes</td>
      <td>True</td>
      <td>116426</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



## Popular Fandoms


```python
# Top 10 most popular fandoms based on cached_count
tags.loc[tags[tags.type == "Fandom"].cached_count.nlargest(10).index]
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
      <th>94292</th>
      <td>136512</td>
      <td>Fandom</td>
      <td>Harry Potter - J. K. Rowling</td>
      <td>True</td>
      <td>361919</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>25</th>
      <td>27</td>
      <td>Fandom</td>
      <td>Supernatural</td>
      <td>True</td>
      <td>310300</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>230408</th>
      <td>414093</td>
      <td>Fandom</td>
      <td>Marvel Cinematic Universe</td>
      <td>True</td>
      <td>240536</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1553725</th>
      <td>3828398</td>
      <td>Fandom</td>
      <td>僕のヒーローアカデミア | Boku no Hero Academia | My Hero ...</td>
      <td>True</td>
      <td>204096</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>680695</th>
      <td>1002903</td>
      <td>Fandom</td>
      <td>방탄소년단 | Bangtan Boys | BTS</td>
      <td>True</td>
      <td>203097</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>168289</th>
      <td>258526</td>
      <td>Fandom</td>
      <td>Teen Wolf (TV)</td>
      <td>True</td>
      <td>172802</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>679946</th>
      <td>1001939</td>
      <td>Fandom</td>
      <td>The Avengers (Marvel Movies)</td>
      <td>True</td>
      <td>157813</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>92059</th>
      <td>133185</td>
      <td>Fandom</td>
      <td>Sherlock (TV)</td>
      <td>True</td>
      <td>151925</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5974</th>
      <td>7266</td>
      <td>Fandom</td>
      <td>Marvel</td>
      <td>True</td>
      <td>147757</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>494603</th>
      <td>758208</td>
      <td>Fandom</td>
      <td>Haikyuu!!</td>
      <td>True</td>
      <td>130918</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



## Popular Relationships


```python
# Top 10 most popular relationships based on cached_count
tags.loc[tags[tags.type == "Relationship"].cached_count.nlargest(10).index]
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
      <th>173025</th>
      <td>264659</td>
      <td>Relationship</td>
      <td>Derek Hale/Stiles Stilinski</td>
      <td>True</td>
      <td>122223</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4700</th>
      <td>5672</td>
      <td>Relationship</td>
      <td>Castiel/Dean Winchester</td>
      <td>True</td>
      <td>111991</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>8900</th>
      <td>11006</td>
      <td>Relationship</td>
      <td>Sherlock Holmes/John Watson</td>
      <td>True</td>
      <td>87435</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>76021</th>
      <td>110293</td>
      <td>Relationship</td>
      <td>James "Bucky" Barnes/Steve Rogers</td>
      <td>True</td>
      <td>77276</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>85</th>
      <td>99</td>
      <td>Relationship</td>
      <td>Draco Malfoy/Harry Potter</td>
      <td>True</td>
      <td>74244</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5973</th>
      <td>7265</td>
      <td>Relationship</td>
      <td>Steve Rogers/Tony Stark</td>
      <td>True</td>
      <td>64923</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>378701</th>
      <td>607596</td>
      <td>Relationship</td>
      <td>Stucky</td>
      <td>False</td>
      <td>54045</td>
      <td>110293.0</td>
    </tr>
    <tr>
      <th>256699</th>
      <td>450395</td>
      <td>Relationship</td>
      <td>Harry Styles/Louis Tomlinson</td>
      <td>True</td>
      <td>48225</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1114701</th>
      <td>1981115</td>
      <td>Relationship</td>
      <td>Jikook</td>
      <td>False</td>
      <td>43327</td>
      <td>5560386.0</td>
    </tr>
    <tr>
      <th>352836</th>
      <td>575567</td>
      <td>Relationship</td>
      <td>Aziraphale/Crowley (Good Omens)</td>
      <td>True</td>
      <td>39319</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



## Popular Freeforms


```python
# Top 10 most popular freeforms based on cached_count
tags.loc[tags[tags.type == "Freeform"].cached_count.nlargest(10).index]
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
      <th>96</th>
      <td>110</td>
      <td>Freeform</td>
      <td>Fluff</td>
      <td>True</td>
      <td>1183065</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>158</th>
      <td>176</td>
      <td>Freeform</td>
      <td>Angst</td>
      <td>True</td>
      <td>813647</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>827</th>
      <td>965</td>
      <td>Freeform</td>
      <td>To Read</td>
      <td>True</td>
      <td>529014</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>63</th>
      <td>76</td>
      <td>Freeform</td>
      <td>Complete</td>
      <td>True</td>
      <td>472403</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1747</th>
      <td>2026</td>
      <td>Freeform</td>
      <td>Smut</td>
      <td>True</td>
      <td>444264</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2065</th>
      <td>2379</td>
      <td>Freeform</td>
      <td>Hurt/Comfort</td>
      <td>True</td>
      <td>415318</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>48</th>
      <td>60</td>
      <td>Freeform</td>
      <td>Romance</td>
      <td>True</td>
      <td>353482</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>74745</th>
      <td>108269</td>
      <td>Freeform</td>
      <td>Read</td>
      <td>True</td>
      <td>351016</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>979</th>
      <td>1160</td>
      <td>Freeform</td>
      <td>Established Relationship</td>
      <td>True</td>
      <td>308719</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>830</th>
      <td>968</td>
      <td>Freeform</td>
      <td>Alternate Universe</td>
      <td>True</td>
      <td>306633</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



# Word Cloud: Character

The [WordCloud](https://github.com/amueller/word_cloud) library allows us to generate wordclouds in python. Here we use character tags as an example. We select the top 50 most popular characters based on their cached_count. To visualize the DataFrame with WordCloud, we use WordCloud().generate_from_frequencies() function.


```python
# Load wordcloud package
%matplotlib inline
import matplotlib.pyplot as plt
from wordcloud import WordCloud
```


```python
# Clear Memory
gc.collect()
```




    48




```python
# Select a subset
# Top 50 character tags
subset = tags.loc[tags[tags.type == "Character"].cached_count.nlargest(50).index]
```


```python
# WordCloud().generate_from_frequencies() requires a dict
# convert subset to a dict

subset = subset.set_index('name').to_dict()['cached_count']
```


```python
# plot
# \ is simply a line breaker
wc = WordCloud(max_words=50, random_state=3, width=1800, height=1200,\
              background_color="white", colormap="tab20").generate_from_frequencies(subset)
```


```python
plt.figure(figsize=(6,4))
plt.imshow(wc, interpolation="bilinear")
plt.axis("off")
plt.title("AO3 Most Popular Characters")
```




    
![png]({{ "assets/output_23_1.png" | relative_url }})
    

