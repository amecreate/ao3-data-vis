---
layout: post
title:  "Rating Tags in Works Part I"
date:   2021-05-25
category: data_cleaning
tags: Python Pandas 
---

In part I, we focus on how to find the rating tags in Tags file and how to add a new rating column in Works file.

* Table of Contents
{:toc}

# Loading File


```python
# Load python libraries
import pandas as pd
```


```python
# Load works file
works= pd.read_csv("/home/pi/Downloads/works-20210226.csv")
```


```python
# Load entire tags file
tags = pd.read_csv("/home/pi/Downloads/tags-20210226.csv")
```


```python
# preview file
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



From the preview, we see that:

- The tags column in **works** contains tag ids for each work, separated by plus sign
- **Tags** file has information about tag ids, types, names, etc

From previous post, we've found what tag **type** looks like:

- Media
- Rating
- ArchiveWarning
- Category
- Character
- Fandom
- Relationship
- Freeform
- UnsortedTag

In this post, we want to find more information about **Rating** tags.

# Rating Tags


```python
# Find rating tags in tags file
rating = tags[tags['type'] == 'Rating']
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
      <th>8</th>
      <td>9</td>
      <td>Rating</td>
      <td>Not Rated</td>
      <td>True</td>
      <td>825385</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>Rating</td>
      <td>General Audiences</td>
      <td>True</td>
      <td>2115153</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10</th>
      <td>11</td>
      <td>Rating</td>
      <td>Teen And Up Audiences</td>
      <td>True</td>
      <td>2272688</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11</th>
      <td>12</td>
      <td>Rating</td>
      <td>Mature</td>
      <td>True</td>
      <td>1151260</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12</th>
      <td>13</td>
      <td>Rating</td>
      <td>Explicit</td>
      <td>True</td>
      <td>1238331</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3686791</th>
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
# Save rating tags in a csv file
rating.to_csv('rating.csv', index=False)
```

There are 5 types of ratings on AO3. The last tag "Teen & Up Audiences" is a duplicate of "Teen And Up Audiences". Because it has a low cached_count compared to others, we discard it in our analysis.

To simplify the data cleaning process in **Rating Tags in Works Part II**, we export the rating DataFrame to a local csv file.

# Tags Column in Works

The tags column in **works** is a long string containing tag ids separated by plus sign. From observation, we find that the first id in the string is most likely a rating id. 

To extract the rating id from the tags column in **works**, we're going to:

- Create a new column named "rating" in **works**
- Extract the first id (which is also the smallest number) from the tags column, add the id to rating column


```python
# Check the type of first row in tags column 
# The first row in tags column is a string

print(works['tags'].iloc[0])
type(works['tags'].iloc[0])
```

    10+414093+1001939+4577144+1499536+110+4682892+21+16





    str




```python
# Check if every row in tags column is string
# Result shows there're NA values in tags column

works[works['tags'].apply(lambda x: isinstance(x,str)) == False]
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
      <th>867445</th>
      <td>2020-09-09</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>63.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2066414</th>
      <td>2019-11-25</td>
      <td>zh</td>
      <td>False</td>
      <td>True</td>
      <td>6593.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4581595</th>
      <td>2017-03-13</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>1012.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# The NA values do not interfere with our analysis
# Drop NA value in tags column

works = works.dropna(subset = ['tags'])
```

To extract the smallest number from a string, we first split the string into a list using .split() method; then we iterate the list in order to change the object type from string to interger; lastly, we're able to select the minimum number from the list with min() function.

To apply the above steps on a Series (the tags column in **works**), we use [pandas.Series.apply](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.apply.html) function.


```python
# Function to find the mimnimum value in the string, and return that value
# First we split the string into a list by the plus sign
# Then we iterate the list, change the object type from string to integer
# Finally we find the minimum value of the list

def find_rating(x):
    return min([int(n) for n in x.split('+')])
```


```python
# Create a new column named 'rating'
# Use apply() to apply a function to each row
# The function returns minimum value in tags column
# The minimun value should be rating id, from observation

works['rating'] = works['tags'].apply(lambda x: find_rating(x))
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
      <th>rating</th>
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
      <td>10</td>
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
      <td>10</td>
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
      <td>10</td>
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
      <td>10</td>
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
      <td>11</td>
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
      <td>13</td>
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
      <td>9</td>
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
      <td>13</td>
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
      <td>13</td>
    </tr>
  </tbody>
</table>
<p>7269690 rows × 8 columns</p>
</div>



We assumed that the first id in the tags string is a rating id, however, extra steps should be taken to check if our assumption is correct. 

From **tags** file, we extracted all correct rating ids. If any row in rating column in **works** falls outside, we'll know there're outliers.


```python
# Check if rating column is indeed rating id
works['rating'].isin(rating['id']).all()
```




    False




```python
# Find the row in rating column that is not rating id
# ~ is negative operator
works[~works['rating'].isin(rating['id'])]
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
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>896014</th>
      <td>2020-09-02</td>
      <td>en</td>
      <td>True</td>
      <td>True</td>
      <td>11852.0</td>
      <td>641+101375+14+640+18417+176+60+20225004+76</td>
      <td>NaN</td>
      <td>14</td>
    </tr>
    <tr>
      <th>896017</th>
      <td>2020-09-02</td>
      <td>en</td>
      <td>True</td>
      <td>True</td>
      <td>3092.0</td>
      <td>641+101375+14+640+18417+60+76</td>
      <td>NaN</td>
      <td>14</td>
    </tr>
    <tr>
      <th>896018</th>
      <td>2020-09-02</td>
      <td>en</td>
      <td>True</td>
      <td>True</td>
      <td>4674.0</td>
      <td>641+101375+14+640+18417+60+20225004+76</td>
      <td>NaN</td>
      <td>14</td>
    </tr>
    <tr>
      <th>896019</th>
      <td>2020-09-02</td>
      <td>en</td>
      <td>True</td>
      <td>True</td>
      <td>1835.0</td>
      <td>641+101375+14+640+18417+176+60+20225004+76</td>
      <td>NaN</td>
      <td>14</td>
    </tr>
    <tr>
      <th>896025</th>
      <td>2020-09-02</td>
      <td>en</td>
      <td>True</td>
      <td>True</td>
      <td>3969.0</td>
      <td>641+101375+14+640+18417+61+60+76</td>
      <td>NaN</td>
      <td>14</td>
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
      <th>5806946</th>
      <td>2015-04-22</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>4111.0</td>
      <td>116+16+1635478+251330+4240694+4205153+270282+1...</td>
      <td>NaN</td>
      <td>16</td>
    </tr>
    <tr>
      <th>6197248</th>
      <td>2014-07-04</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>875.0</td>
      <td>23+14+1285452+1433626+2332773+2332776+663+2332779</td>
      <td>NaN</td>
      <td>14</td>
    </tr>
    <tr>
      <th>6299245</th>
      <td>2014-04-14</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>2395.0</td>
      <td>23+16+586439+55409+7267+17067+30326+501842+237...</td>
      <td>NaN</td>
      <td>16</td>
    </tr>
    <tr>
      <th>6404542</th>
      <td>2014-01-10</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>1350.0</td>
      <td>23+14+721553+976131+897092+989133</td>
      <td>NaN</td>
      <td>14</td>
    </tr>
    <tr>
      <th>6456251</th>
      <td>2013-11-23</td>
      <td>en</td>
      <td>False</td>
      <td>True</td>
      <td>1271.0</td>
      <td>21+14+872785+993549+1009491+984790+984786+1212...</td>
      <td>NaN</td>
      <td>14</td>
    </tr>
  </tbody>
</table>
<p>488 rows × 8 columns</p>
</div>



There are 488 works with no rating. This is actually a [know issue](https://otwarchive.atlassian.net/browse/AO3-6065) that the volunteers are actively working on behind-the-curtain. Thus, we simply drop these works from our data set for now. 


```python
# Drop works with no rating
works = works[works['rating'].isin(rating['id'])]
```

Now we have a file that has all the rating information extracted to a single column. We'll create graphs based on the information in the next post.
