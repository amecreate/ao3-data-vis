---
layout: post
title:  "English Works Trend and Seasonality"
date:   2021-05-16
categories: data_analysis visualization
tags: Python Seaborn statsmodels
---

![png]({{ "assets/0516seasonality/output_22_0.png" | relative_url }})

In this section, we analyze the trend and seasonality of users' posting habits. 

The number of works posted to the Archive is time-series data, which is a set of obervations obtained in different time periods. We are not going into details of time-series models in this section. Instead, we'll briefly showcase two components of time-series analysis: autocorrelation, and decomposing trend and seasonality from the data set.

* Table of Contents
{:toc}

# Loading File


```python
# Load Python library
import pandas as pd

# Load file
path="/home/pi/Downloads/works-20210226.csv"
chunker = pd.read_csv(path, chunksize=10000)
works = pd.concat(chunker, ignore_index=True)
```

# Data Cleaning

Since majority of the works posted to AO3 are in English, we choose English works in the data set for analysis. The data set contains the creation date of each work, and we're going to sum up the number of works posted per month from 2008 to 2021. All of the Python functions used here are discussed in previous blog posts.


```python
# Select two columns that we're interested in
# Select English works for analysis
eng = works[['creation date','language']][works['language'] == 'en']

# Drop NA values
eng = eng.dropna()

# Preview of the DataFrame
eng
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
<p>6587693 rows × 2 columns</p>
</div>




```python
# Make sure date column is in datetime format
eng['creation date'] = pd.to_datetime(eng['creation date'])

# Group by monthly posting 
# Use pd.Grouper because it aggregates throughout the years
eng = eng.groupby([pd.Grouper(key='creation date',freq='1M')]).count()

# Preview of the DataFrame
eng
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
      <th>language</th>
    </tr>
    <tr>
      <th>creation date</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2008-09-30</th>
      <td>928</td>
    </tr>
    <tr>
      <th>2008-10-31</th>
      <td>480</td>
    </tr>
    <tr>
      <th>2008-11-30</th>
      <td>337</td>
    </tr>
    <tr>
      <th>2008-12-31</th>
      <td>239</td>
    </tr>
    <tr>
      <th>2009-01-31</th>
      <td>499</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>2020-10-31</th>
      <td>141120</td>
    </tr>
    <tr>
      <th>2020-11-30</th>
      <td>122796</td>
    </tr>
    <tr>
      <th>2020-12-31</th>
      <td>154417</td>
    </tr>
    <tr>
      <th>2021-01-31</th>
      <td>147813</td>
    </tr>
    <tr>
      <th>2021-02-28</th>
      <td>137125</td>
    </tr>
  </tbody>
</table>
<p>150 rows × 1 columns</p>
</div>



# Plotting The Data
## Line Plot


```python
# Import libraries
# Top line is Jupyter Notebook specific

%matplotlib inline

import matplotlib.pyplot as plt
import seaborn as sns
```


```python
# Line plot using seaborn library
# Orignal data
ax = sns.lineplot(data=eng)

# Aesthetics 
ax.set_title('Works Posted Per Month \n 2008-2021')
ax.legend(title='Language', labels=['English'])
plt.ylabel('works')
```




    Text(0, 0.5, 'works')




    
![png]({{ "assets/0516seasonality/output_9_1.png" | relative_url }})
    


## Seasonal Plot


```python
# Add a year column
eng_annual = eng.copy().reset_index()
eng_annual['year'] = eng_annual['creation date'].dt.year

# Rename columns
eng_annual.columns = ['creation date', 'works', 'year']

# Preview data
eng_annual
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
      <th>works</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2008-09-30</td>
      <td>928</td>
      <td>2008</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2008-10-31</td>
      <td>480</td>
      <td>2008</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2008-11-30</td>
      <td>337</td>
      <td>2008</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2008-12-31</td>
      <td>239</td>
      <td>2008</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2009-01-31</td>
      <td>499</td>
      <td>2009</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>145</th>
      <td>2020-10-31</td>
      <td>141120</td>
      <td>2020</td>
    </tr>
    <tr>
      <th>146</th>
      <td>2020-11-30</td>
      <td>122796</td>
      <td>2020</td>
    </tr>
    <tr>
      <th>147</th>
      <td>2020-12-31</td>
      <td>154417</td>
      <td>2020</td>
    </tr>
    <tr>
      <th>148</th>
      <td>2021-01-31</td>
      <td>147813</td>
      <td>2021</td>
    </tr>
    <tr>
      <th>149</th>
      <td>2021-02-28</td>
      <td>137125</td>
      <td>2021</td>
    </tr>
  </tbody>
</table>
<p>150 rows × 3 columns</p>
</div>




```python
# Visualizaion
ax = sns.lineplot(data=eng_annual, x=eng_annual['creation date'].dt.month, y='works', hue='year', ci=None)

# Aethetics
plt.xlabel('Month')
plt.ylabel('Works')
plt.title('Works Posted Per Month \n 2008-2021')
```




    Text(0.5, 1.0, 'Works Posted Per Month \n 2008-2021')




    
![png]({{ "assets/0516seasonality/output_12_1.png" | relative_url }})
    


## Boxplot Distribution


```python
# Monthly Posting
ax = sns.boxplot(data=eng_annual, x=eng_annual['creation date'].dt.month, y='works')

# Aethetics
plt.xlabel('Month')
plt.ylabel('Works')
plt.title('Works Posted Per Month \n 2008-2021')
```




    Text(0.5, 1.0, 'Works Posted Per Month \n 2008-2021')




    
![png]({{ "assets/0516seasonality/output_14_1.png" | relative_url }})
    



```python
# Annual Posting
ax = sns.boxplot(data=eng_annual, x=eng_annual['creation date'].dt.year, y='works')

# Aethetics
plt.xlabel('Year')
plt.ylabel('Works')
plt.title('Works Posted Per Year \n 2008-2021')
```




    Text(0.5, 1.0, 'Works Posted Per Year \n 2008-2021')




    
![png]({{ "assets/0516seasonality/output_15_1.png" | relative_url }})
    


# Autocorrelation

Autocorrelation shows the correlation of the data in one period to its occurrence in the previous period. When data are both trended and seasonal, we will see a pattern in our ACF (AutoCorrelation Function) graph.


```python
# Import Statsmodels library
import statsmodels.api as sm
```


```python
# AFC plot
ax = sm.graphics.tsa.plot_acf(eng, lags=37)

#Aesthetics
plt.xlabel('Lag')
plt.ylabel('Correlation')
```




    Text(0, 0.5, 'Correlation')




    
![png]({{ "assets/0516seasonality/output_18_1.png" | relative_url }})
    


The ACF decreases when the lags increase, indicating that the data is trended. 

# Decomposing A Time Series Data

Time series data comprise three components: trend, seasonality, and residual (noise). An additive decomposition assumes that: data = trend + seasonality + residual; alternatively, a multiplicative decomposition assumes that: data = trend x seasonality x residual.


```python
# Import Statsmodels library
from statsmodels.tsa.seasonal import seasonal_decompose

# Adjust figure size
plt.rcParams['figure.figsize'] = [7, 5]
```


```python
# Additive Decomposition
result_add = seasonal_decompose(eng, model='additive')
ax=result_add.plot()
```


    
![png]({{ "assets/0516seasonality/output_22_0.png" | relative_url }})
    



```python
# Multiplicative Decomposition
result_mul = seasonal_decompose(eng, model='multiplicative')
ax = result_mul.plot()
```


    
![png]({{ "assets/0516seasonality/output_23_0.png" | relative_url }})
    

