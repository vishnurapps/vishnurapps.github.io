---
title:  "Introduction to Pandas Dataframe"
date:   2019-05-23
layout: single
author_profile: true
comments: true
tags: 
---

Data frame is a main object in pandas. It is used to represent data with rows and columns. Data frame is a datastructure that represent the data in tabular or excel spread sheet.

#H3 Creating a dataframe

We can do this by reading data from a csv file

```
python
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


Another way is to read data from list of tuples.

```
python
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


