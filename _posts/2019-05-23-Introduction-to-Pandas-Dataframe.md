---
title:  "Introduction to Pandas Dataframe"
date:   2019-05-23
layout: single
author_profile: true
comments: true
tags: 
---

Data frame is a main object in pandas. It is used to represent data with rows and columns. Data frame is a datastructure that represent the data in tabular or excel spread sheet.

### Creating a dataframe

We can create a dataframe in multiple ways. The simplest way is by reading data from a csv file

```python
import pandas as pd
df = pd.read_csv("weather_data.csv")   #read weather.csv data
df
```

|   | day      | temperature | windspeed | event |
|---|----------|-------------|-----------|-------|
| 0 | 1/1/2017 | 32          | 6         | Rain  |
| 1 | 1/2/2017 | 35          | 7         | Sunny |
| 2 | 1/3/2017 | 28          | 2         | Snow  |
| 3 | 1/4/2017 | 24          | 7         | Snow  |
| 4 | 1/5/2017 | 32          | 4         | Rain  |
| 5 | 1/6/2017 | 31          | 2         | Sunny |


If we have the data as a list of tuples, we can conver that to a dataframe.

```python
#list of tuples

weather_data = [('1/1/2017', 32, 6, 'Rain'),
                ('1/2/2017', 35, 7, 'Sunny'),
                ('1/3/2017', 28, 2, 'Snow'),
                ('1/4/2017', 24, 7, 'Snow'),
                ('1/5/2017', 32, 4, 'Rain'),
                ('1/6/2017', 31, 2, 'Sunny')
               ]
df = pd.DataFrame(weather_data, columns=['day', 'temperature', 'windspeed', 'event'])
df
```


|   | day      | temp | windspeed | event |
|---|----------|------|-----------|-------|
| 0 | 1/1/2017 | 32   | 6         | Rain  |
| 1 | 1/2/2017 | 35   | 7         | Sunny |
| 2 | 1/3/2017 | 28   | 2         | Snow  |
| 3 | 1/4/2017 | 24   | 7         | Snow  |
| 4 | 1/5/2017 | 32   | 4         | Rain  |
| 5 | 1/6/2017 | 31   | 2         | Sunny |


To see the dimension of the dataframe we can use `df.shape`

```python
df.shape
```

This will return `(6,4)` as these are 6 columns and 4 rows. The index is not counted in column.

We can use `df.head` to see the top 5 rows of the dataframe.

```python
df.head()
```

|   | day      | temperature | windspeed | event |
|---|----------|-------------|-----------|-------|
| 0 | 1/1/2017 | 32          | 6         | Rain  |
| 1 | 1/2/2017 | 35          | 7         | Sunny |
| 2 | 1/3/2017 | 28          | 2         | Snow  |
| 3 | 1/4/2017 | 24          | 7         | Snow  |
| 4 | 1/5/2017 | 32          | 4         | Rain  |

Similar to `df.head()` there is a `df.tail()` which shows the last 5 elements

```python
df.tail()
```

|   | day      | temperature | windspeed | event |
|---|----------|-------------|-----------|-------|
| 1 | 1/2/2017 | 35          | 7         | Sunny |
| 2 | 1/3/2017 | 28          | 2         | Snow  |
| 3 | 1/4/2017 | 24          | 7         | Snow  |
| 4 | 1/5/2017 | 32          | 4         | Rain  |
| 5 | 1/6/2017 | 31          | 2         | Sunny |

We can see that index 0 is not present in the above table.

if we want to see the column header, we can use the `df.columns`

```python
df.columns
```

`Index(['day', 'temperature', 'windspeed', 'event'], dtype='object')`

### Slicing 
```python
df[2:5]
```

|   | day      | temperature | windspeed | event |
|---|----------|-------------|-----------|-------|
| 2 | 1/3/2017 | 28          | 2         | Snow  |
| 3 | 1/4/2017 | 24          | 7         | Snow  |
| 4 | 1/5/2017 | 32          | 4         | Rain  |

This will return the rows from 2nd row till 4th(5-1) row

If we want to print a particular column, we can use the `df.columnname`

```python
df.day      #print particular column data
```
`0    1/1/2017
1    1/2/2017
2    1/3/2017
3    1/4/2017
4    1/5/2017
5    1/6/2017
Name: day, dtype: object`

```python
df['day'] #df.day (both are same)
```
This will also gets us the column day

We can print multiple rows using this method.

```python
df[['day', 'event']]
```
|   | day      | event |
|---|----------|-------|
| 0 | 1/1/2017 | Rain  |
| 1 | 1/2/2017 | Sunny |
| 2 | 1/3/2017 | Snow  |
| 3 | 1/4/2017 | Snow  |
| 4 | 1/5/2017 | Rain  |
| 5 | 1/6/2017 | Sunny |
