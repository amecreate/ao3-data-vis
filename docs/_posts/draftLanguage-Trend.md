



# Language Trend

Let's look at the language options on AO3.


```python
import pandas as pd
```


```python
# Load File

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


