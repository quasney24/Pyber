

1) The City type has a direct connection with how many drivers, rides, & fares with Urban being the greater of all. 


```python
#import dependencies
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
import seaborn as sns
from scipy import stats
import os
```


```python
city_file = os.path.join("raw_data", "city_data.csv")
ride_file = os.path.join("raw_data" , "ride_data.csv")

raw_city_data_df = pd.read_csv(city_file)
raw_ride_data_df = pd.read_csv(ride_file)




```


```python
#merge tables on city
#raw_city_data_df.drop_duplicates(inplace = True)

merged_raw_df = raw_city_data_df.merge(raw_ride_data_df, on = 'city')

merged_raw_df.head()
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




```python
grouped_merged_raw = merged_raw_df.groupby(["city"])

grouped_merged_raw
#Average fare per city
average_fare_city = grouped_merged_raw["fare"].mean()
#average_fare_city

#total rides per city
tRides_city = grouped_merged_raw["city"].count()
#tRides_city


#total number of drivers per city
tDrivers_city = grouped_merged_raw["driver_count"].count()
tDrivers_city

#city type
city_type = grouped_merged_raw["type"].unique()
city_type



#dataframe time
scatter_plot_df = pd.DataFrame({"Average Fare": average_fare_city,
                                "Total Rides": tRides_city,
                                "Total Drivers": tDrivers_city,
                                "City Type": city_type})
scatter_plot_df.head()
                    

rural = scatter_plot_df[scatter_plot_df["City Type"]== "Rural"]
suburban = scatter_plot_df[scatter_plot_df["City Type"] == "Suburban"]
urban = scatter_plot_df[scatter_plot_df["City Type"] == "Urban"]


```


```python

plt.grid(color="white")

#urban
urban_plot = plt.scatter(urban['Total Rides'], urban['Average Fare'], s = urban['Total Drivers']*6, color = 'lightcoral', edgecolor = 'black',
        label = 'Urban', alpha = .75)

suburban_plot = plt.scatter(suburban['Total Rides'], suburban['Average Fare'], s = suburban['Total Drivers']*6, color = 'lightblue', edgecolor = 'black',
        label = 'Suburban', alpha = .75)


rural_plot = plt.scatter(rural['Total Rides'], rural['Average Fare'], s = rural['Total Drivers']*6, color = 'gold', edgecolor = 'black',
        label = 'Rural', alpha = .75)

plt.xlim(0, 40)
plt.ylim(15,52)

lgnd = plt.legend(loc='best')
lgnd.legendHandles[0]._sizes = [70]
lgnd.legendHandles[1]._sizes = [70]
lgnd.legendHandles[2]._sizes = [70]

plt.title("Pyber Ride Sharing Data (2016)")
plt.xlabel("Total Number of Rides (per city)")
plt.ylabel("Average Fare ($)")

plt.show()
```


![png](output_5_0.png)



```python
labels = ["Rural", "Surburban", "Urban"]
colors = ["Gold", "Lightskyblue", "Lightcoral"]
explode=[0,0,.1]

pi_df = merged_raw_df.groupby(["type"])["fare"].sum()

plt.pie(pi_df, explode=explode, labels =labels, colors = colors,autopct= '%1.1f%%', shadow=True, startangle=120)

plt.title('% of Total Fares by City Type')

plt.axis("equal")

plt.savefig("fare_type.png")
plt.show()
#pi_df_final
```


![png](output_6_0.png)



```python
pi_2_df = merged_raw_df.groupby(["type"])["ride_id"].count()

plt.pie(pi_2_df,explode=explode, labels =labels, colors = colors,autopct= '%1.1f%%', shadow=True, startangle=120)

plt.title('% of Rides per City Type')

plt.axis("equal")

#save figure
plt.savefig("rides_total.png")
plt.show()
```


![png](output_7_0.png)



```python
drivers_df = raw_city_data_df.groupby(["type"])["driver_count"].sum()

plt.pie(drivers_df,explode=explode, labels =labels, colors = colors,autopct= '%1.1f%%', shadow=True, startangle=120)

plt.title('% of Drivers per City Type')

plt.axis("equal")


#save figure
plt.savefig("drivers_total.png")
plt.show()
```


![png](output_8_0.png)



```python

```
