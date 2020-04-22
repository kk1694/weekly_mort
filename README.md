# Weekly Mortality
> A repo for collecting weekly mortality


<img src="nbs/../total_deaths.png">

Excess mortality (how much larger the number of 2020 deaths are vs previous years) is an important source of information for undertanding COVID-19. 

This data is available from many statistics bureaus. However, as far as I'm aware, it's aggregated nowhere. This is unfortunate, as researchers/journalists/random folks who want to use it have to fish it together from multiple sources.

Moreover, the data needs to be structured properly. Releases are often in human-readable excel spreadsheets, but that's not very helpful for code. Hence, I aim to put everything into easy-to-use .csv files.

Similar projects have been done by the [Economist](https://www.economist.com/graphic-detail/2020/04/16/tracking-covid-19-excess-deaths-across-countries) or [The New York Times](https://www.nytimes.com/interactive/2020/04/21/world/coronavirus-missing-deaths.html). However, their data & code are not publicly available. 

For now, I've only processed the UK and the Netherlands, but aim to include more countries soon.

The repo itself uses [nbdev](https://github.com/fastai/nbdev). All data processing is done in jupyter notebooks. The code is not very clean, let me know if there are any bugs or inaccuracies.

## Data

The data in the `dataset/` folder. It contains 3 files.

### total_deaths.csv

This file tabulates deaths by country, week for 2017 - 2020. Note that for some countries, week 0 is fractional, so it's best to start any analysis from week 1.

```python
import pandas as pd
from IPython.core.display import HTML

HTML(pd.read_csv('../dataset/total_deaths.csv').head().to_html())
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Week</th>
      <th>Deaths2017</th>
      <th>Deaths2018</th>
      <th>Deaths2019</th>
      <th>Deaths2020</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Netherlands</td>
      <td>0</td>
      <td>469.0</td>
      <td>3343.0</td>
      <td>2606.0</td>
      <td>2242.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Netherlands</td>
      <td>1</td>
      <td>3568.0</td>
      <td>3359.0</td>
      <td>3262.0</td>
      <td>3364.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Netherlands</td>
      <td>2</td>
      <td>3637.0</td>
      <td>3364.0</td>
      <td>3150.0</td>
      <td>3151.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Netherlands</td>
      <td>3</td>
      <td>3487.0</td>
      <td>3322.0</td>
      <td>3178.0</td>
      <td>3040.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Netherlands</td>
      <td>4</td>
      <td>3626.0</td>
      <td>3403.0</td>
      <td>3143.0</td>
      <td>3157.0</td>
    </tr>
  </tbody>
</table>



### excess_deaths.csv

Some countries give larger granularity than simply total deaths. They might break down the numbers by age group, region, etc. Below are the subgroups for each country.

Note that each country usually doesn't have all combinations for all subgroups. For example, the UK breaks down deaths by age, and also by geography; but it doesn't break them down by age *and* geography.

```python
from weekly_mort.collect import SUMMARY

HTML(SUMMARY)
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Year</th>
      <th>Age</th>
      <th>Sex</th>
      <th>Region</th>
      <th>Condition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Netherlands</td>
      <td>[2017, 2018, 2019, 2020]</td>
      <td>[Total, 0-64, 65-79, 80+]</td>
      <td>[Total, Male, Female]</td>
      <td>[Total]</td>
      <td>[Total]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>United Kingdom</td>
      <td>[2020, 2017, 2018, 2019]</td>
      <td>[Total, 01-14, 15-44, 45-64, 65-74, 75-84, 85+, Under 1 year]</td>
      <td>[Total, Female, Male]</td>
      <td>[Total, North East, North West, Yorkshire and The Humber, East Midlands, West Midlands, East, London, South East, South West, Wales]</td>
      <td>[Total, Respiratory]</td>
    </tr>
  </tbody>
</table>



The data is already in an easy-to-use format.

```python
df = pd.read_csv('../dataset/excess_deaths.csv')

HTML(df.head().to_html())
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Week</th>
      <th>Year</th>
      <th>Age</th>
      <th>Sex</th>
      <th>Region</th>
      <th>Condition</th>
      <th>Deaths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Netherlands</td>
      <td>0</td>
      <td>2017</td>
      <td>Total</td>
      <td>Total</td>
      <td>Total</td>
      <td>Total</td>
      <td>469</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Netherlands</td>
      <td>1</td>
      <td>2017</td>
      <td>Total</td>
      <td>Total</td>
      <td>Total</td>
      <td>Total</td>
      <td>3568</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Netherlands</td>
      <td>2</td>
      <td>2017</td>
      <td>Total</td>
      <td>Total</td>
      <td>Total</td>
      <td>Total</td>
      <td>3637</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Netherlands</td>
      <td>3</td>
      <td>2017</td>
      <td>Total</td>
      <td>Total</td>
      <td>Total</td>
      <td>Total</td>
      <td>3487</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Netherlands</td>
      <td>4</td>
      <td>2017</td>
      <td>Total</td>
      <td>Total</td>
      <td>Total</td>
      <td>Total</td>
      <td>3626</td>
    </tr>
  </tbody>
</table>



```python
import matplotlib.pyplot as plt
import datetime

uk = df[df.Country == 'United Kingdom']
uk = uk[(uk.Age != 'Total') & (uk.Region == 'Total') & (uk.Condition == 'Total')]

uk.loc[uk.Age == 'Under 1 year', 'Age'] = '0-1'
ages = list(uk.Age.unique())
ages.sort()

fig, axes = plt.subplots(7, 3, figsize=(15, 8), sharex=True, sharey='row')

for i, age in enumerate(ages):
    for j, sex in enumerate(uk.Sex.unique()):
        sub = uk[(uk.Age == age) & (uk.Sex == sex)]
        sub = sub[sub.Week <= 5 + datetime.date.today().isocalendar()[1]]
        sub2020 = sub[sub.Year == 2020]
        sub2019 = sub[sub.Year == 2019]
        axes[i, j].plot(sub2019.Week, sub2019.Deaths, color = 'b', label='Deaths in 2019')
        axes[i, j].plot(sub2020.Week, sub2020.Deaths, color = 'r', label='Deaths in 2020')
        
        if i == 0:  axes[i, j].set_title(sex)
        if j == 0:  axes[i, j].set_ylabel(age)
        if i == len(ages) - 1:  axes[i, j].set_xlabel('Week')
            
fig.suptitle('United Kingdom Deaths by Age and Sex');
```


![png](docs/images/output_12_0.png)


### weekly_dates.csv

Week numbers differ by country: in the UK, the week ends on a Friday, in the Netherlands, on a Sunday. They also differ in how they treat the first week of the year.

```python
dates = pd.read_csv('../dataset/week_dates.csv')
dates[dates.Week == 0]
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
      <th>Country</th>
      <th>Year</th>
      <th>Week</th>
      <th>Start</th>
      <th>End</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Netherlands</td>
      <td>2017</td>
      <td>0</td>
      <td>2017-01-01</td>
      <td>2017-01-01</td>
    </tr>
    <tr>
      <th>53</th>
      <td>Netherlands</td>
      <td>2018</td>
      <td>0</td>
      <td>2018-01-01</td>
      <td>2018-01-07</td>
    </tr>
    <tr>
      <th>106</th>
      <td>Netherlands</td>
      <td>2019</td>
      <td>0</td>
      <td>2019-01-01</td>
      <td>2019-01-06</td>
    </tr>
    <tr>
      <th>159</th>
      <td>Netherlands</td>
      <td>2020</td>
      <td>0</td>
      <td>2020-01-01</td>
      <td>2020-01-05</td>
    </tr>
    <tr>
      <th>212</th>
      <td>United Kingdom</td>
      <td>2017</td>
      <td>0</td>
      <td>2016-12-31</td>
      <td>2017-01-06</td>
    </tr>
    <tr>
      <th>265</th>
      <td>United Kingdom</td>
      <td>2018</td>
      <td>0</td>
      <td>2017-12-30</td>
      <td>2018-01-05</td>
    </tr>
    <tr>
      <th>318</th>
      <td>United Kingdom</td>
      <td>2019</td>
      <td>0</td>
      <td>2018-12-29</td>
      <td>2019-01-04</td>
    </tr>
    <tr>
      <th>371</th>
      <td>United Kingdom</td>
      <td>2020</td>
      <td>0</td>
      <td>2019-12-28</td>
      <td>2020-01-03</td>
    </tr>
  </tbody>
</table>
</div>



```python
dates
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
      <th>Country</th>
      <th>Year</th>
      <th>Week</th>
      <th>Start</th>
      <th>End</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Netherlands</td>
      <td>2017</td>
      <td>0</td>
      <td>2017-01-01</td>
      <td>2017-01-01</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Netherlands</td>
      <td>2017</td>
      <td>1</td>
      <td>2017-01-02</td>
      <td>2017-01-08</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Netherlands</td>
      <td>2017</td>
      <td>2</td>
      <td>2017-01-09</td>
      <td>2017-01-15</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Netherlands</td>
      <td>2017</td>
      <td>3</td>
      <td>2017-01-16</td>
      <td>2017-01-22</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Netherlands</td>
      <td>2017</td>
      <td>4</td>
      <td>2017-01-23</td>
      <td>2017-01-29</td>
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
      <th>419</th>
      <td>United Kingdom</td>
      <td>2020</td>
      <td>48</td>
      <td>2020-11-28</td>
      <td>2020-12-04</td>
    </tr>
    <tr>
      <th>420</th>
      <td>United Kingdom</td>
      <td>2020</td>
      <td>49</td>
      <td>2020-12-05</td>
      <td>2020-12-11</td>
    </tr>
    <tr>
      <th>421</th>
      <td>United Kingdom</td>
      <td>2020</td>
      <td>50</td>
      <td>2020-12-12</td>
      <td>2020-12-18</td>
    </tr>
    <tr>
      <th>422</th>
      <td>United Kingdom</td>
      <td>2020</td>
      <td>51</td>
      <td>2020-12-19</td>
      <td>2020-12-25</td>
    </tr>
    <tr>
      <th>423</th>
      <td>United Kingdom</td>
      <td>2020</td>
      <td>52</td>
      <td>2020-12-26</td>
      <td>2020-12-31</td>
    </tr>
  </tbody>
</table>
<p>424 rows × 5 columns</p>
</div>



## Country Sources

### Netherlands

Their API wasn't working at the time of writing, however the data access is pretty straightforward on their [website](1052).

### United Kingdom

The [Office for National Statistics](https://www.ons.gov.uk/peoplepopulationandcommunity/birthsdeathsandmarriages/deaths/datasets/weeklyprovisionalfiguresondeathsregisteredinenglandandwales) provides data for many population subgroups with a 10 day delay.

### United States

I looked at the data from the [CDC Influenza Surveillance](https://gis.cdc.gov/grasp/fluview/mortality.html) (see [nbs/_021_US.ipynb](nbs/_021_US.ipynb)). 

However, I choose not to include the data, as the counts were too low -- probably not all deaths were processed yet (despite a ">100%" flag saying otherwise). Also, there were strange artifacts, like the deaths for New York City being higher than the deaths for New York.

If anyone knows an alternative data source for the US, please let me know.

### Sweden TODO

[Source](https://www.scb.se/en/finding-statistics/statistics-by-subject-area/population/population-composition/population-statistics/#_Tablesandgraphs)

## How to contribute

If you know data sources for weekly deaths for other countries, please let me know. Either comment on github, or send an email to krisztiankovacs@fastmail.com

If you feel like contributing, you can also create a notebook that standardizes the mortality information. Here are the rough steps:
1. Create a subfolder under `_downloads` with the country name, and place all downloaded material in there.
1. Create a subfolder under `_processed` with the country name, and add two .csv files (see folders for Netherlands and UK):
  - week_dates.csv: A spreadsheet with 4 columns giving the year, week number, week start date, week end date
  - deaths.csv: A spreadsheet with minimum 3 columns: Year, Week, Deaths. If available, you can also include subgroups by age, sex, region, condition.
  
Ideally, the whole process is automated in a notebook, from downloading the files, reformatting them, and saving the above two .csv files. Sometimes however, it's easier to download manually. In that case, clearly outline the steps in the notebook on how to update the data in the future, and include and subsequent processing step.
