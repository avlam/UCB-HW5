
# Pyber Ride Sharing
Pyber Exercise
written by: A. Lam

# Analysis:
- Observation 1: Assuming that fares are directly related to distance driven, it would make sense that urban cities would have a higher quantity of rides per city with shorter fares. Similarly, rural areas would have less frequent rides spanning much larger distances, resulting in higher average fares.
- Observation 2: The quantity of rides contributed by Urban cities offsets the fact that most of them have much lower fares than rural areas, as evidenced by the bubble chart and pie chart on total fares.
- Observation 3: Port James has a surprisingly high number of rides uncharacteristic of any other city in the dataset. (edit: Turns out, Point James had been double-entered with two different driver counts in city_data. Judgement call to determine the true number is to use the first entry and discard the second. The implication of the action constrains analysis impact to driver count, as without the correction rides are double-counted.)

# Setup


```python
# Modules
import os
import pandas as pd
import matplotlib.pyplot as plt
```


```python
# import raw data
city_data_path = os.path.join('..','Instructions','Pyber','raw_data','city_data.csv')
ride_data_path = os.path.join('..','Instructions','Pyber','raw_data','ride_data.csv')
city_data = pd.read_csv(city_data_path)
ride_data = pd.read_csv(ride_data_path)
```


```python
print('# Unique Cities: ' + str(len(city_data['city'].unique())) + 
      '\n# Cities Entered: ' + str(len(city_data['city'])))
city_data = city_data.drop_duplicates(subset = ['city'],keep='first')
print('After Cleaning')
print('# Unique Cities: ' + str(len(city_data['city'].unique())) + 
      '\n# Cities Entered: ' + str(len(city_data['city'])))
city_data.head()
```

    # Unique Cities: 125
    # Cities Entered: 126
    After Cleaning
    # Unique Cities: 125
    # Cities Entered: 125





<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Nguyenbury</td>
      <td>8</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>East Douglas</td>
      <td>12</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>West Dawnfurt</td>
      <td>34</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rodriguezburgh</td>
      <td>52</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
ride_data.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sarabury</td>
      <td>2016-01-16 13:49:27</td>
      <td>38.35</td>
      <td>5403689035038</td>
    </tr>
    <tr>
      <th>1</th>
      <td>South Roy</td>
      <td>2016-01-02 18:42:34</td>
      <td>17.49</td>
      <td>4036272335942</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Wiseborough</td>
      <td>2016-01-21 17:35:29</td>
      <td>44.18</td>
      <td>3645042422587</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Spencertown</td>
      <td>2016-07-31 14:53:22</td>
      <td>6.87</td>
      <td>2242596575892</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Nguyenbury</td>
      <td>2016-07-09 04:42:44</td>
      <td>6.28</td>
      <td>1543057793673</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Join of data sources
all_data = pd.merge(city_data,ride_data,on='city')
all_data.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-08-19 04:27:52</td>
      <td>5.51</td>
      <td>6246006544795</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-04-17 06:59:50</td>
      <td>5.54</td>
      <td>7466473222333</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-05-04 15:06:07</td>
      <td>30.54</td>
      <td>2140501382736</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-01-25 20:44:56</td>
      <td>12.08</td>
      <td>1896987891309</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-08-09 18:19:47</td>
      <td>17.91</td>
      <td>8784212854829</td>
    </tr>
  </tbody>
</table>
</div>



# Bubble Plot of Ride Sharing Data
Include:
- Average Fare per City (y)
- Total Rides per City (x)
- Total Drivers per City (Bubble Size)
- City Type (Color)


```python
city_types = all_data['type'].unique()
colors = ['gold','lightskyblue','lightcoral']
# add color assignment
c_assign = pd.DataFrame({'type' : city_types,
                        'color' : colors})
all_data_colored = pd.merge(all_data,c_assign,on='type')
all_data_colored.head()
# Create groupby over city
city_group = all_data_colored.groupby(['city'])
bubble_data = pd.DataFrame({
    'Average Fare' : round(city_group['fare'].mean(),2),
    'Total Rides' : city_group['ride_id'].count(),
    'Total Drivers' : city_group['driver_count'].mean(), # should all be the same value per city
    'City Type' : city_group['type'].min(), # should be identical per city
    'color' : city_group['color'].min() # should be identical per city
    })
bubble_data.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Fare</th>
      <th>City Type</th>
      <th>Total Drivers</th>
      <th>Total Rides</th>
      <th>color</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alvarezhaven</th>
      <td>23.93</td>
      <td>Urban</td>
      <td>21</td>
      <td>31</td>
      <td>gold</td>
    </tr>
    <tr>
      <th>Alyssaberg</th>
      <td>20.61</td>
      <td>Urban</td>
      <td>67</td>
      <td>26</td>
      <td>gold</td>
    </tr>
    <tr>
      <th>Anitamouth</th>
      <td>37.32</td>
      <td>Suburban</td>
      <td>16</td>
      <td>9</td>
      <td>lightskyblue</td>
    </tr>
    <tr>
      <th>Antoniomouth</th>
      <td>23.62</td>
      <td>Urban</td>
      <td>21</td>
      <td>22</td>
      <td>gold</td>
    </tr>
    <tr>
      <th>Aprilchester</th>
      <td>21.98</td>
      <td>Urban</td>
      <td>49</td>
      <td>19</td>
      <td>gold</td>
    </tr>
  </tbody>
</table>
</div>




```python
# bubble_data.loc[bubble_data['City Type'] == city_types[0]]['Average Fare']
plt.figure(figsize = (10,10))
sf = 10 # scaling factor
for i in range(len(bubble_data['City Type'].unique())):
    ctype = bubble_data['City Type'].unique()[i]
    x = bubble_data.loc[bubble_data['City Type'] == ctype]['Total Rides']
    y = bubble_data.loc[bubble_data['City Type'] == ctype]['Average Fare']
    size = bubble_data.loc[bubble_data['City Type'] == ctype]['Total Drivers']
    plt.scatter(x, y, s = sf*size, 
                color = bubble_data.loc[bubble_data['City Type'] == ctype]['color'], 
                                      label = city_types[i], edgecolor = 'k')

plt.legend()
plt.title('Pyber Ride Sharing Data')
plt.xlabel('Total Rides Per City')
plt.ylabel('Average Fare per City')
plt.text(0,20,'Note: \nBubbles are sized by Total Drivers per City')
plt.show()
```


![png](Pyber_avl_files/Pyber_avl_10_0.png)



```python
# identify outlier
bubble_data.sort_values(by=['Total Rides'],ascending=False).head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Fare</th>
      <th>City Type</th>
      <th>Total Drivers</th>
      <th>Total Rides</th>
      <th>color</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Port Johnstad</th>
      <td>25.88</td>
      <td>Urban</td>
      <td>22</td>
      <td>34</td>
      <td>gold</td>
    </tr>
    <tr>
      <th>Swansonbury</th>
      <td>27.46</td>
      <td>Urban</td>
      <td>64</td>
      <td>34</td>
      <td>gold</td>
    </tr>
    <tr>
      <th>South Louis</th>
      <td>27.09</td>
      <td>Urban</td>
      <td>12</td>
      <td>32</td>
      <td>gold</td>
    </tr>
    <tr>
      <th>Port James</th>
      <td>31.81</td>
      <td>Suburban</td>
      <td>15</td>
      <td>32</td>
      <td>lightskyblue</td>
    </tr>
    <tr>
      <th>Alvarezhaven</th>
      <td>23.93</td>
      <td>Urban</td>
      <td>21</td>
      <td>31</td>
      <td>gold</td>
    </tr>
  </tbody>
</table>
</div>



# Total Fares by City Type


```python
group_type = all_data_colored.groupby(['type'])
plt.pie(group_type['fare'].sum(),
        labels = group_type['fare'].sum().keys(), 
        colors = group_type['color'].min(), 
        explode = [0,0,0.1],
       autopct = '%1.1f%%')
plt.title('% of Total Fares by City Type')
plt.axis('equal')
plt.show()
```


![png](Pyber_avl_files/Pyber_avl_13_0.png)


# Total Rides by City Type


```python
group_type = bubble_data.groupby(['City Type'])
plt.pie(group_type['Total Rides'].sum(),
        labels = group_type['Total Rides'].sum().keys(), 
        colors = group_type['color'].min(), 
        explode = [0,0,0.1],
       autopct = '%1.1f%%')
plt.title('% of Total Rides by City Type')
plt.axis('equal')
plt.show()
```


![png](Pyber_avl_files/Pyber_avl_15_0.png)


# Total Drivers by City Type


```python
group_type = all_data_colored.groupby(['type'])
plt.pie(group_type['driver_count'].sum(),
        labels = group_type['driver_count'].sum().keys(), 
        colors = group_type['color'].min(), 
        explode = [0,0,0.1],
       autopct = '%1.1f%%')
plt.title('% of Total Drivers by City Type')
plt.axis('equal')
plt.show()
```


![png](Pyber_avl_files/Pyber_avl_17_0.png)

