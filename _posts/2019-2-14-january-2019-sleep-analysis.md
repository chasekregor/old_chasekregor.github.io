---
layout: post
title: January 2019 Sleep Analysis
comments: true
redirect_from: "2019-2-14-january-2019-sleep-analysis"
permalink: january-2019-sleep-analysis
---


### Introduction:
I am a big fan on the [Quantified Self Movement](https://en.wikipedia.org/wiki/Quantified_self). Collecting and routinely analyzing my own personal data has enabled me with the ability to understand myself in a much more objective manner. I use a variety of applications and devices to track various data points about myself. In this post I attempt to analyze my sleep last month.

### Questions:
- What was my average sleep for the month?
- What was the distribution and variation in my sleep?
- What was the difference in sleep on weekdays versus weekends?
- What was the difference in sleep per day of the week?



```python
%%capture
import pandas as pd
import numpy as np
import altair as alt
import seaborn as sns
%matplotlib widget
import pandas_profiling
import datetime
import scipy
%matplotlib inline
```


```python
#importing data from my health app using http://quantifiedself.com/qs-access-app/
sleep = pd.read_csv('data/sleep/Sleep Analysis Jan 2019.csv')

#converting date column into datetime
sleep['In bed start'] = pd.to_datetime(sleep['In bed start'])
sleep['In bed Finish'] = pd.to_datetime(sleep['In bed Finish'])

#setting the index to the date
sleep = sleep.set_index(pd.DatetimeIndex(sleep['In bed Finish']))

#ENTER THIS MONTH'S NUMBER HERE TO FILTER FOR THIS MONTH & Year
sleep = sleep[sleep.index.month.isin([1]) & sleep.index.year.isin([2019])]

#sleep = sleep.sort_values(['In bed Finish'], ascending=[True])

#getting only the columns I want
sleep = sleep.iloc[:,0:3]
sleep.head()
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
      <th>In bed start</th>
      <th>In bed Finish</th>
      <th>Minutes in bed</th>
    </tr>
    <tr>
      <th>In bed Finish</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-01 09:27:00</th>
      <td>2019-01-01 02:17:00</td>
      <td>2019-01-01 09:27:00</td>
      <td>430.000000</td>
    </tr>
    <tr>
      <th>2019-01-02 08:31:00</th>
      <td>2019-01-02 01:21:00</td>
      <td>2019-01-02 08:31:00</td>
      <td>430.150632</td>
    </tr>
    <tr>
      <th>2019-01-03 08:20:00</th>
      <td>2019-01-03 00:03:00</td>
      <td>2019-01-03 08:20:00</td>
      <td>497.354967</td>
    </tr>
    <tr>
      <th>2019-01-04 09:00:00</th>
      <td>2019-01-04 02:16:00</td>
      <td>2019-01-04 09:00:00</td>
      <td>403.071597</td>
    </tr>
    <tr>
      <th>2019-01-05 10:31:00</th>
      <td>2019-01-05 02:17:00</td>
      <td>2019-01-05 10:31:00</td>
      <td>494.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
sleep['Hours in Bed'] = sleep['Minutes in bed'] / 60.0
sleep['bedtime'] = sleep['In bed start'].dt.hour + sleep['In bed start'].dt.minute / 60.0
```


```python
sleep.head()
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
      <th>In bed start</th>
      <th>In bed Finish</th>
      <th>Minutes in bed</th>
      <th>Hours in Bed</th>
      <th>bedtime</th>
    </tr>
    <tr>
      <th>In bed Finish</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-01 09:27:00</th>
      <td>2019-01-01 02:17:00</td>
      <td>2019-01-01 09:27:00</td>
      <td>430.000000</td>
      <td>7.166667</td>
      <td>2.283333</td>
    </tr>
    <tr>
      <th>2019-01-02 08:31:00</th>
      <td>2019-01-02 01:21:00</td>
      <td>2019-01-02 08:31:00</td>
      <td>430.150632</td>
      <td>7.169177</td>
      <td>1.350000</td>
    </tr>
    <tr>
      <th>2019-01-03 08:20:00</th>
      <td>2019-01-03 00:03:00</td>
      <td>2019-01-03 08:20:00</td>
      <td>497.354967</td>
      <td>8.289249</td>
      <td>0.050000</td>
    </tr>
    <tr>
      <th>2019-01-04 09:00:00</th>
      <td>2019-01-04 02:16:00</td>
      <td>2019-01-04 09:00:00</td>
      <td>403.071597</td>
      <td>6.717860</td>
      <td>2.266667</td>
    </tr>
    <tr>
      <th>2019-01-05 10:31:00</th>
      <td>2019-01-05 02:17:00</td>
      <td>2019-01-05 10:31:00</td>
      <td>494.000000</td>
      <td>8.233333</td>
      <td>2.283333</td>
    </tr>
  </tbody>
</table>
</div>




```python
#https://github.com/tuchandra/sleep-analysis/blob/master/spring-sleep-analysis.ipynb

dates = sleep['In bed Finish']
time_in_bed = []

for i in sleep['Minutes in bed']:
    time_in_bed.append(i / 60.0)

df = pd.DataFrame(time_in_bed)
df.columns = ['Hours in Bed']


df.plot()
```








![png](../assets/output_5_1.png)


## What was my average sleep for the month?
Almost exactly at 8 hours.


```python
df.describe().transpose()
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
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Hours in Bed</th>
      <td>30.0</td>
      <td>8.019936</td>
      <td>1.358089</td>
      <td>4.69825</td>
      <td>7.167294</td>
      <td>7.961942</td>
      <td>8.551076</td>
      <td>11.6</td>
    </tr>
  </tbody>
</table>
</div>



## What was the distribution and variation in my sleep?


```python
df.plot.hist(bins = 8, range = (3, 11))
```









![png](../assets/output_9_1.png)


## What was the difference in sleep on weekdays versus weekends?


```python
sleep['day_of_week'] = sleep.index.weekday
sleep['day_type'] = sleep['day_of_week'].apply(lambda x: 'Weekend' if x >= 5 else 'Weekday')
```


```python
sleep.boxplot(column = 'Hours in Bed', by = 'day_type', positions = [2, 1],
           vert = True, widths = 0.5)
```









![png](../assets/output_12_1.png)


## What was the difference in sleep per day of the week?


```python
# Group dataframe by weekday vs. weekend

df_weekdays = sleep[sleep.day_of_week < 5]
df_weekend = sleep[sleep.day_of_week >= 5]
```


```python
# Add a label for day name, to make the boxplot more readable
days = {0: 'Monday', 1: 'Tuesday', 2: 'Wednesday', 3: 'Thursday', 4: 'Friday',
        5: 'Saturday', 6: 'Sunday'}
sleep['day_name'] = sleep['day_of_week'].apply(lambda x: days[x])

sleep.boxplot(column = 'Hours in Bed', by = 'day_name', positions = [5, 1, 6, 7, 4, 2, 3], vert = True)
```









![png](../assets/output_15_1.png)



```python
sleep.plot.scatter(x = 'bedtime', y = 'Hours in Bed' )
```









![png](../assets/output_16_1.png)


## Final Thoughts and Next Steps:

January was a really great month for me sleep wise. I have pretty poor sleep habits as you can see by the times. However, to average 8 hours a night for a whole month is a pretty big win for me.


